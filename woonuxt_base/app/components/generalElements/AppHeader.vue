<script setup lang="ts">
const links = [
  { name: 'HOME', link: '/', prefetch: true },
  { name: 'INTRODUCING', link: '/#about'},
  { name: 'UPCOMING EVENTS', link: '/#champions' },
  { name: 'COACHES', link: '/#COACHES' },
  { name: 'COMPETITIONS', link: '/#COMPETITIONS' },
  { name: 'MEDIA', link: '/#MEDIA' },
  { name: 'REGISTRATION', link: 'https://registration.movedsa.com/', external: true },
  // { name: 'WEBSHOP', link: '/sub' },
  // { name: 'APP', link: '/sub' },
  { name: 'CONTACT', link: '/#CONTACT', prefetch: true }, // Javítva
];

const activeSection = ref<string | null>('HOME'); // Alapértelmezett HOME aktív

const handleLinkClick = (linkName: string, linkUrl: string, external: boolean = false) => {
  if (external) {
    // External linkek esetén új ablakban nyitjuk meg
    window.open(linkUrl, '_blank');
    return;
  }
  
  if (linkUrl.startsWith('/#')) {
    activeSection.value = linkName;
  } else if (linkUrl === '/') {
    activeSection.value = 'HOME';
  } else {
    activeSection.value = null;
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
            <!-- External linkek -->
            <a
              v-if="link.external"
              :href="link.link"
              target="_blank"
              rel="noopener noreferrer"
              @click="handleLinkClick(link.name, link.link, true)"
              class="text-gray-700 hover:text-[#FA3DFF] transition-colors duration-200 text-sm font-unbounded font-light cursor-pointer"
            >
              {{ link.name }}
            </a>
            
            <!-- Internal linkek -->
            <NuxtLink
              v-else
              :to="link.link"
              :prefetch="link.prefetch"
              @click="handleLinkClick(link.name, link.link)"
              :class="{
                'font-medium': activeSection === link.name,
                'font-light': activeSection !== link.name
              }"
              class="text-gray-700 hover:text-[#FA3DFF] transition-colors duration-200 text-sm font-unbounded"
            >
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

/* Aktív szekció stílus (kézi kezelés) */
.font-medium {
  font-weight: 500 !important;
}

.font-light {
  font-weight: 300 !important;
}

/* Minden link alapértelmezett súlya */
:deep(a) {
  font-weight: 300;
}

@media screen and (max-width: 767px) {
  .menu[data-v-7cac02f6] {
    width: 85%;
  }
}
</style>