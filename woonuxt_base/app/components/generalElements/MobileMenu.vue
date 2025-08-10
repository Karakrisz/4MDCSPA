<script setup lang="ts">
const { toggleMobileMenu } = useHelpers();

const links = [
  { text: 'HOME', to: '/', prefetch: false },
  { text: 'INTRODUCING', to: '/sub', prefetch: false },
  { text: 'COACHES', to: '/sub', prefetch: false },
  { text: 'COURSES', to: '/sub', prefetch: false },
  { text: 'COMPETITIONS', to: '/sub', prefetch: false },
  { text: 'MEDIA', to: '/sub', prefetch: false },
  { text: 'WEBSHOP', to: '/sub', prefetch: false },
  { text: 'APP', to: '/sub', prefetch: false },
  { text: 'CONTACT', to: '/sub', prefetch: false },
];
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
        <NuxtLink
          v-for="link in links"
          :key="link.to"
          :to="link.to"
          :class="['cursor-pointer text-lg transition hover:text-primary font-unbounded']"
          active-class="text-primary"
          :prefetch="link.prefetch">
          {{ link.text }}
        </NuxtLink>
      </nav>
    </div>
  </div>
</template>
