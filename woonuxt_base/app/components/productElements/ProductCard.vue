<script setup lang="ts">
const route = useRoute();
const { storeSettings } = useAppConfig();
const { addToCart, isUpdatingCart } = useCart();

const props = defineProps({
  node: { type: Object as PropType<Product>, required: true },
  index: { type: Number, default: 1 },
});

// example: ?filter=pa_color[green,blue],pa_size[large]
const filterQuery = ref(route.query?.filter as string);
const paColor = ref(filterQuery.value?.split('pa_color[')[1]?.split(']')[0]?.split(',') || []);

// watch filterQuery
watch(
  () => route.query,
  () => {
    filterQuery.value = route.query.filter as string;
    paColor.value = filterQuery.value?.split('pa_color[')[1]?.split(']')[0]?.split(',') || [];
  },
);

const mainImage = computed<string>(() => props.node?.image?.producCardSourceUrl || props.node?.image?.sourceUrl || '/images/placeholder.jpg');
const imagetoDisplay = computed<string>(() => {
  if (paColor.value.length) {
    const activeColorImage = props.node?.variations?.nodes.filter((variation) => {
      const hasMatchingAttributes = variation.attributes?.nodes.some((attribute) => paColor.value.some((color) => attribute?.value?.includes(color)));
      const hasMatchingSlug = paColor.value.some((color) => variation.slug?.includes(color));
      return hasMatchingAttributes || hasMatchingSlug;
    });
    if (activeColorImage?.length) return activeColorImage[0]?.image?.producCardSourceUrl || activeColorImage[0]?.image?.sourceUrl || mainImage.value;
  }
  return mainImage.value;
});

// Biztonságos slug kezelés
const safeSlug = computed(() => props.node.slug || '');
const productUrl = computed(() => safeSlug.value ? `/product/${decodeURIComponent(safeSlug.value)}` : '#');
</script>

<template>
  <div class="relative rounded-2xl overflow-hidden bg-black min-h-[600px] group cursor-pointer">
    <!-- Háttérkép a termékképből -->
    <div 
      class="absolute inset-0 bg-cover bg-center transition-transform duration-500 group-hover:scale-105"
      :style="`background-image: url('${imagetoDisplay}')`"
    >
      <!-- Fekete gradient overlay a szöveg olvashatóságáért -->
      <div class="absolute inset-0 bg-gradient-to-t from-black/80 via-black/20 to-transparent"></div>
    </div>

    <!-- Sale Badge -->
    <SaleBadge :node class="absolute top-4 right-4 z-10" />

    <!-- Tartalom -->
    <div class="relative z-10 p-8 h-full flex flex-col justify-between">
      <!-- Fejléc -->
      <div>
        <h3 class="text-white text-2xl font-bold mb-4 leading-tight">
          {{ node.name }}
        </h3>
      </div>

      <!-- Alsó rész: Leírás, ár és gombok -->
      <div class="space-y-6">
        <!-- Termék leírás -->
        <div 
          class="text-white/90 text-sm leading-relaxed line-clamp-4"
          v-html="node.shortDescription"
        />

        <!-- Méret/specifikáció (ha van) -->
        <div v-if="node.attributes" class="text-white/80 text-sm font-medium">
          <!-- Itt lehet megjeleníteni a termék specifikációit -->
          <!-- Például: Mérete: 3 m × 1,5 m × 0,76 m -->
        </div>

        <!-- Ár -->
        <div class="mb-6">
          <ProductPrice 
            class="text-lg font-bold text-white" 
            :sale-price="node.salePrice" 
            :regular-price="node.regularPrice" 
          />
        </div>

        <!-- Gombok -->
        <div class="flex gap-4">
          <!-- Részletek gomb -->
          <NuxtLink 
            :to="productUrl" 
            :title="node.name"
            class="flex-1 px-6 py-3 border-2 border-white/30 text-white font-medium rounded-lg hover:border-white/50 hover:bg-white/10 transition-all duration-200 text-center"
          >
            Részletek
          </NuxtLink>

          <!-- Kosárba gomb -->
          <NuxtLink 
            v-if="node.slug" 
            :to="productUrl" 
            :title="node.name"
            class="flex-1 px-6 py-3 bg-[#FF5D19] hover:bg-[#E54A14] text-white font-medium rounded-lg transition-all duration-200 flex items-center justify-center gap-2 group"
          >
            <span>Kosárba</span>
            <svg class="w-4 h-4 group-hover:translate-x-1 transition-transform" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 3h2l.4 2M7 13h10l4-8H5.4m0 0L7 13m0 0l-1.6 8M7 13v6a2 2 0 002 2h6a2 2 0 002-2v-6" />
            </svg>
          </NuxtLink>
        </div>
      </div>
    </div>

    <!-- Hover overlay - extra interaktivitás -->
    <div class="absolute inset-0 bg-black/20 opacity-0 group-hover:opacity-100 transition-opacity duration-300 pointer-events-none"></div>
  </div>
</template>

<style scoped>
/* Line clamp utility a leírás korlátozásához */
.line-clamp-4 {
  display: -webkit-box;
  -webkit-line-clamp: 4;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
</style>