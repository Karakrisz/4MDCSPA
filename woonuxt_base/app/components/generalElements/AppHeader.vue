<script setup lang="ts">
const links = [
  { name: 'HOME', link: '/', prefetch: true },
  { name: 'INTRODUCING', link: '/sub' },
  { name: 'COACHES', link: '/sub' },
  { name: 'COURSES', link: '/sub' },
  { name: 'COMPETITIONS', link: '/sub' },
  { name: 'MEDIA', link: '/sub' },
  { name: 'WEBSHOP', link: '/sub' },
  { name: 'APP', link: '/sub' },
  { name: 'CONTACT', link: '/sub', prefetch: true },
];
</script>

<template>
  <header class="z-50 top-0 bg-white">
    <div class="menu container flex justify-between items-center">

      <!-- Menü és kosár jobb oldalt -->
      <div class="flex items-center justify-center w-full">
        <!-- Mobile menu trigger -->
        <MenuTrigger class="lg:hidden" />
        
        <!-- Desktop navigáció -->
        <div class="flex items-center justify-between w-full max-w-6xl hidden lg:flex">
          <NuxtLink 
            v-for="link in links" 
            :key="link.name" 
            :to="link.link" 
            class="text-gray-700 hover:text-[#FF5D19] transition-colors duration-200 text-sm font-unbounded"
          >
            {{ link.name }}
          </NuxtLink>
        </div>

        <!-- Jobb oldali gombok -->
        <!-- <div class="flex gap-4">
          <SignInLink />
          <div class="bg-[#FF5D19] rounded-md">
            <CartTrigger class="text-white" />
          </div>
        </div> -->
      </div>
    </div>
  </header>
</template>

<style scoped>
header {
  background-color: transparent;
  height: 0;
}
.menu {
  margin: auto;
  margin-top: 3.5em;
  background: #fff;
  padding: 1.3em 2em;
}

:deep(.cart-trigger) {
  color: white;
  background: transparent;
}

:deep(.cart-trigger):hover {
  background: rgba(255, 255, 255, 0.1);
}

/* Aktív link stílus */
:deep(.router-link-active) {
  font-weight: 500 !important;
}

/* Default link stílus */
:deep(a:not(.router-link-active)) {
  font-weight: 300;
}
</style>