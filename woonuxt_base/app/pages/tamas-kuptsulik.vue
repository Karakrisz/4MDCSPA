<script setup>
import { ref, onMounted } from 'vue';

// Reactive state
const isVisible = ref(false);

// Trigger animations on mount
onMounted(() => {
  setTimeout(() => {
    isVisible.value = true;
  }, 100);
});

// Meta tags for SEO
useHead({
  title: 'Tamás Kuptsulik - Academy Director & Coach | Move Dance Sport Academy',
  meta: [
    {
      name: 'description',
      content:
        "Meet Tamás Kuptsulik, Academy Director and Coach at Move Dance Sport Academy. World Championship winning coach and founder of Dubai's premier dance sport training facility.",
    },
  ],
});
</script>

<template>
  <div class="min-h-screen bg-black text-white">
    <!-- Hero Section -->
    <div class="container mx-auto p-12 lg:py-[190px]">
      <div class="grid grid-cols-1 lg:grid-cols-2 gap-6 lg:gap-8 items-center" :class="{ 'animate-slide-right': isVisible }">
        <!-- Image Section -->
        <div class="relative pt-[50px] lg:pt-0">
          <div class="relative w-full max-w-sm mx-auto lg:max-w-none">
            <div class="relative overflow-hidden rounded-lg">
              <img
                src="/img/tamas.webp"
                alt="Tamás Kuptsulik"
                class="w-full h-[600px] lg:h-[1200px] object-cover object-top transform hover:scale-105 transition-transform duration-500 pt-[50px] lg:pt-0" />
              <!-- Gradient Overlay -->
              <div
                class="absolute inset-0 bg-gradient-to-tr from-[#ff29f7]/20 to-[#24d3ee]/20 opacity-0 hover:opacity-100 transition-opacity duration-300"></div>
            </div>
          </div>
        </div>

        <!-- Content Section -->
        <div class="space-y-4">
          <!-- Header -->
          <div class="space-y-1">
            <h1
              class="text-2xl lg:text-4xl font-bold tracking-wide animate-gradient-text bg-gradient-to-r from-[#ff29f7] to-[#24d3ee] bg-clip-text text-transparent">
              TAMÁS
            </h1>
            <h2
              class="text-2xl lg:text-4xl font-bold tracking-wide animate-gradient-text bg-gradient-to-r from-[#ff29f7] to-[#24d3ee] bg-clip-text text-transparent">
              KUPTSULIK
            </h2>
            <div class="space-y-0.5 mt-3">
              <p class="text-base lg:text-lg text-gray-300 font-light">ACADEMY DIRECTOR</p>
              <p class="text-base lg:text-lg text-gray-300 font-light">COACH</p>
            </div>
          </div>

          <!-- Contact Info -->
          <div
            class="bg-gradient-to-r from-[#ff29f7] to-[#24d3ee] p-3 rounded-lg transform hover:scale-105 transition-all duration-300 hover:shadow-lg hover:shadow-[#ff29f7]/25">
            <p class="text-white font-semibold text-base">+36 70 617 6179</p>
            <p class="text-white font-semibold text-sm">info@movedsa.com</p>
          </div>

          <!-- Content Sections -->
          <div class="space-y-6">
            <!-- Professional Journey -->
            <div class="transform transition-all duration-500 hover:translate-x-2">
              <h3 class="text-lg font-bold mb-2 text-[#24d3ee]">Professional Journey</h3>
              <p class="text-gray-300 leading-relaxed text-sm">
                My first encounter with Acrobatic Rock and Roll came at age 5. Throughout my 13-year competitive career, I achieved multiple national
                championship titles before transitioning to sports management and professional coaching in 2010. For the past 5 years, I have served as the head
                of the Fashion Dance Division at the Hungarian Dance Sport Federation, while also directing one of Europe's largest dance sport federations,
                4MDC, which unites nearly 150 coaches and 10,000 competitive dancers.
              </p>
            </div>

            <!-- International Success -->
            <div class="transform transition-all duration-500 hover:translate-x-2">
              <h3 class="text-lg font-bold mb-2 text-[#ff29f7]">International Success</h3>
              <p class="text-gray-300 leading-relaxed text-sm">
                My competitive dancers have achieved remarkable results both domestically and internationally. Beyond multiple World Cup and World Championship
                podium placements, we secured a World Championship title in the adult category in 2022 and a European Championship gold medal in 2024.
              </p>
            </div>

            <!-- Founder Info -->
            <div class="transform transition-all duration-500 hover:translate-x-2">
              <h3 class="text-lg font-bold mb-2 text-[#24d3ee]">Founder of Move Dance Sport Academy</h3>
              <p class="text-gray-300 leading-relaxed text-sm">
                In 2024, I established Move Dance Sport Academy with the vision of elevating Acrobatic Rock and Roll competitive sport through collaboration
                with the industry's finest professionals and modern methodology. Move represents the highest standard of dance sport training in Dubai and the
                United Arab Emirates, offering international competitive careers and professional development opportunities for our athletes.
              </p>
            </div>

            <!-- Mission -->
            <div class="transform transition-all duration-500 hover:translate-x-2">
              <p class="text-gray-300 leading-relaxed text-sm">
                Our mission is to establish Move Dance Sport Academy as the world's leading Acrobatic Rock and Roll club, pioneering a new chapter in the
                sport's history both in the Emirates and on the international stage.
              </p>
            </div>

            <!-- CTA Button -->
            <div class="pt-4">
              <NuxtLink to="veronika-lengyel"
                class="bg-gradient-to-r from-[#ff29f7] to-[#24d3ee] text-white font-semibold text-[9px] lg:text-[16px] py-3 px-8 transform hover:scale-105 transition-all duration-300 hover:shadow-lg hover:shadow-[#ff29f7]/25 active:scale-95">
                CONTINUE TO VERONIKA LENGYEL'S PAGE
              </NuxtLink>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
@keyframes slideLeft {
  from {
    opacity: 0;
    transform: translateX(-100px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

@keyframes slideRight {
  from {
    opacity: 0;
    transform: translateX(100px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

@keyframes gradientShift {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}

.animate-slide-left {
  animation: slideLeft 0.8s ease-out forwards;
}

.animate-slide-right {
  animation: slideRight 1s ease-out forwards;
}

.animate-gradient-text {
  background-size: 200% 200%;
  animation: gradientShift 3s ease infinite;
}

/* Initial hidden state */
.animate-slide-right:not(.animate-slide-right) {
  opacity: 0;
  transform: translateX(100px);
}

/* Custom scrollbar */
::-webkit-scrollbar {
  width: 8px;
}

::-webkit-scrollbar-track {
  background: #000;
}

::-webkit-scrollbar-thumb {
  background: linear-gradient(to bottom, #ff29f7, #24d3ee);
  border-radius: 4px;
}

::-webkit-scrollbar-thumb:hover {
  background: linear-gradient(to bottom, #24d3ee, #ff29f7);
}
</style>
