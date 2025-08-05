<script setup lang="ts">
const links = [
  { name: 'Mi az a teqball?', link: '/', prefetch: true },
  { name: 'Termékeink', link: '/' },
  { name: 'Kapcsolat', link: '/', prefetch: true },
];
</script>

<template>
  <header class="z-50 sticky top-0 bg-white py-4">
    <div class="menu container flex justify-between items-center gap-10">
      <!-- Logo bal oldalt -->
      <NuxtLink to="/" class="shrink-0">
        <div class="flex items-center">
          <span class="text-2xl font-bold text-[#FF5D19] pl-2">
            <!-- <NuxtImg src="/img/logo.svg" class="h-5" /> -->
             logo helye
          </span>
        </div>
      </NuxtLink>

      <!-- Menü és kosár jobb oldalt -->
      <div class="flex items-center gap-8">
        <!-- Mobile menu trigger -->
        <MenuTrigger class="lg:hidden" />
        
        <!-- Desktop navigáció -->
        <div class="space-x-8 lg:flex items-center hidden">
          <NuxtLink 
            v-for="link in links" 
            :key="link.name" 
            :to="link.link" 
            class="text-gray-700 hover:text-[#FF5D19] transition-colors duration-200"
          >
            {{ link.name }}
          </NuxtLink>
        </div>

        <!-- Jobb oldali gombok -->
        <div class="flex gap-4">
          <SignInLink />
          
          <!-- CartTrigger wrapper a narancssárga háttérhez -->
          <div class="bg-[#FF5D19] rounded-md">
            <CartTrigger class="text-white" />
          </div>
        </div>
      </div>
    </div>
  </header>
</template>

<style scoped>
.menu {
  margin: auto;
  margin-top: 2em;
  margin-bottom: 1em;
  border-radius: 1.5em;
  background: var(--Light-Grey, #F3F3F3);
  padding: 1.3em 2em;
}

:deep(.cart-trigger) {
  color: white;
  background: transparent;
}

:deep(.cart-trigger):hover {
  background: rgba(255, 255, 255, 0.1);
}
</style>