<script setup lang="ts">
const { toggleMobileMenu } = useHelpers();

const links = [
  { text: 'HOME', to: '/', prefetch: false },
  { text: 'INTRODUCING', to: '/#about', prefetch: false },
  { text: 'UPCOMING EVENTS', to: '/#champions', prefetch: false },
  { text: 'COACHES', to: '/#COACHES', prefetch: false },
  { text: 'COMPETITIONS', to: '/#COMPETITIONS', prefetch: false },
  { text: 'MEDIA', to: '/#MEDIA', prefetch: false },
  { text: 'REGISTRATION', to: 'https://registration.movedsa.com/', external: true },
  // { text: 'WEBSHOP', to: '/sub', prefetch: false },
  // { text: 'APP', to: '/sub', prefetch: false },
  { text: 'CONTACT', to: '/#CONTACT', prefetch: false }, // Javítva
];

const handleLinkClick = (link: any) => {
  if (link.external) {
    // External linkek esetén új ablakban nyitjuk meg
    window.open(link.to, '_blank');
    toggleMobileMenu(false); // Bezárjuk a mobile menüt
    return;
  }
  
  // Internal linkek esetén normál navigáció
  toggleMobileMenu(false); // Bezárjuk a mobile menüt
};
</script>

<template>
  <div class="bg-white flex flex-col max-w-lg shadow-lg top-0 bottom-0 left-0 w-11/12 z-50 fixed overflow-x-hidden p-20">
    <Icon
      name="ion:close-outline"
      class="absolute p-1 rounded-lg shadow-lg top-6 left-6 md:left-8 cursor-pointer hover:text-primary transition"
      size="34"
      @click="toggleMobileMenu(false)" />

    <div class="flex flex-col items-center justify-center h-full gap-10">
      <!-- <ProductSearch class="h-fit w-full relative" /> -->
      <nav class="flex flex-col items-center justify-center gap-10">
        <template v-for="link in links" :key="link.to">
          <!-- External linkek -->
          <a
            v-if="link.external"
            :href="link.to"
            target="_blank"
            rel="noopener noreferrer"
            @click="handleLinkClick(link)"
            class="cursor-pointer text-lg transition hover:text-primary font-unbounded"
          >
            {{ link.text }}
          </a>
          
          <!-- Internal linkek -->
          <NuxtLink
            v-else
            :to="link.to"
            :class="['cursor-pointer text-lg transition hover:text-primary font-unbounded']"
            active-class="text-primary"
            :prefetch="link.prefetch"
            @click="handleLinkClick(link)"
          >
            {{ link.text }}
          </NuxtLink>
        </template>
      </nav>
    </div>
  </div>
</template>