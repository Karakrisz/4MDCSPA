<script setup lang="ts">
const links = [
  { name: 'HOME', link: '/', prefetch: true },
  { name: 'INTRODUCING', link: '/#about'}, 
  { name: 'CHAMPIONS', link: '/#champions' },
  { name: 'COACHES', link: '/sub' },
  { name: 'COMPETITIONS', link: '/sub' },
  { name: 'MEDIA', link: '/sub' },
  { name: 'WEBSHOP', link: '/sub' },
  { name: 'APP', link: '/sub' },
  { name: 'CONTACT', link: '/sub', prefetch: true },
];

const activeLink = ref<string | null>(null);

const handleLinkClick = (link: any, event: Event) => {
  // Ha anchor link és ugyanazon az oldalon vagyunk
  if (link.link.startsWith('/#')) {
    event.preventDefault();
    const elementId = link.link.substring(2); // Eltávolítjuk a "/#" részt
    const element = document.getElementById(elementId);
    
    if (element) {
      // Aktív link beállítása
      activeLink.value = link.name;
      
      element.scrollIntoView({ 
        behavior: 'smooth',
        block: 'start'
      });
    }
  } else {
    // Normál navigáció esetén töröljük az aktív anchor linket
    activeLink.value = null;
  }
};
</script>

<template>
  <header class="z-50 top-0 bg-white">
    <div class="menu container flex justify-between items-center">
      <!-- Menü és kosár jobb oldalt -->
      <div class="flex items-center lg:justify-center w-full">
        <!-- Mobile menu trigger -->
        <MenuTrigger class="lg:hidden ml-5" />

        <!-- Desktop navigáció -->
        <div class="flex items-center justify-between w-full max-w-6xl hidden lg:flex">
          <template v-for="link in links" :key="link.name">
            <!-- Anchor linkek (smooth scroll) -->
            <button
              v-if="link.link.startsWith('/#')"
              @click="handleLinkClick(link, $event)"
              :class="{
                'font-medium': activeLink === link.name,
                'font-light': activeLink !== link.name
              }"
              class="text-gray-700 hover:text-[#FF5D19] transition-colors duration-200 text-sm font-unbounded bg-transparent border-none cursor-pointer">
              {{ link.name }}
            </button>
            
            <!-- Normál navigáció linkek -->
            <NuxtLink
              v-else
              :to="link.link"
              @click="handleLinkClick(link, $event)"
              class="text-gray-700 hover:text-[#FF5D19] transition-colors duration-200 text-sm font-unbounded">
              {{ link.name }}
            </NuxtLink>
          </template>
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
/* Smooth scroll CSS támogatás */
html {
  scroll-behavior: smooth;
}

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

/* Aktív anchor link stílus (kézi kezelés) */
.font-medium {
  font-weight: 500 !important;
}

.font-light {
  font-weight: 300 !important;
}

/* Router aktív link stílus (csak normál navigációnál) */
:deep(.router-link-active) {
  font-weight: 500 !important;
}

/* Default link stílus */
:deep(a:not(.router-link-active)) {
  font-weight: 300;
}

@media screen and (max-width: 767px) {
  .menu[data-v-7cac02f6] {
    width: 85%;
  }
}
</style>