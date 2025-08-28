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
  title: 'Bonita Lengyel Mészárosné - Coach | Move Dance Sport Academy',
  meta: [
    {
      name: 'description',
      content:
        "Meet Bonita Lengyel Mészárosné, Coach at Move Dance Sport Academy. International competitor with expertise in Acrobatic Rock and Roll and Hip-Hop choreography.",
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
                src="/img/boni.webp"
                alt="Bonita Lengyel Mészárosné"
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
              class="text-xl lg:text-3xl font-bold tracking-wide animate-gradient-text bg-gradient-to-r from-[#ff29f7] to-[#24d3ee] bg-clip-text text-transparent">
              BONITA
            </h1>
            <h2
              class="text-xl lg:text-3xl font-bold tracking-wide animate-gradient-text bg-gradient-to-r from-[#ff29f7] to-[#24d3ee] bg-clip-text text-transparent">
              LENGYEL
            </h2>
            <h3
              class="text-xl lg:text-3xl font-bold tracking-wide animate-gradient-text bg-gradient-to-r from-[#ff29f7] to-[#24d3ee] bg-clip-text text-transparent">
              MÉSZÁROSNÉ
            </h3>
            <div class="space-y-0.5 mt-3">
              <p class="text-base lg:text-lg text-gray-300 font-light">COACH</p>
            </div>
          </div>

          <!-- Contact Info -->
          <div
            class="bg-gradient-to-r from-[#ff29f7] to-[#24d3ee] p-3 rounded-lg transform hover:scale-105 transition-all duration-300 hover:shadow-lg hover:shadow-[#ff29f7]/25">
            <p class="text-white font-semibold text-base">+971 56 140 5898</p>
            <p class="text-white font-semibold text-sm">bonita.meszaros@movedsa.com</p>
          </div>

          <!-- Content Sections -->
          <div class="space-y-6">
            <!-- Professional Journey -->
            <div class="transform transition-all duration-500 hover:translate-x-2">
              <h3 class="text-lg font-bold mb-2 text-[#24d3ee]">Professional Journey</h3>
              <p class="text-gray-300 leading-relaxed text-sm">
                I entered the world of Acrobatic Rock and Roll as a hobby dancer and quickly advanced to the sport's highest level. My career transformed after meeting Mészáros Gábor, who became my husband and dance partner. My Hip-Hop background significantly enriched my movement repertoire.
              </p>
              <p class="text-gray-300 leading-relaxed text-sm mt-3">
                Competing in Main Class Freestyle presented significant challenges, requiring mastery of complex acrobatics, saltos, and twists. Training with coaches and dancers from Spartak Moscow, we achieved multiple Top 5 placements in Free Style formations. Over the past decade, I've competed in more than 25 countries, gaining extensive international experience.
              </p>
            </div>

            <!-- Coaching Methodology -->
            <div class="transform transition-all duration-500 hover:translate-x-2">
              <h3 class="text-lg font-bold mb-2 text-[#ff29f7]">Coaching Methodology</h3>
              <p class="text-gray-300 leading-relaxed text-sm">
                I create choreographies for couples and formations, blending Acrobatic Rock and Roll with Hip-Hop elements. My unique approach produces fresh, dynamic, and innovative performances that showcase each dancer's individual talent.
              </p>
            </div>

            <!-- At Move Dance Sport Academy Dubai -->
            <div class="transform transition-all duration-500 hover:translate-x-2">
              <h3 class="text-lg font-bold mb-2 text-[#24d3ee]">At Move Dance Sport Academy Dubai</h3>
              <p class="text-gray-300 leading-relaxed text-sm">
                My goal is to develop both technical excellence and artistic expression in our competitors. I now bring my international experience and diverse dance expertise to Dubai, nurturing future champions while preserving their passion for dance.
              </p>
            </div>

            <!-- CTA Button -->
            <div class="pt-4">
              <NuxtLink to="patrik-toma"
                class="bg-gradient-to-r from-[#ff29f7] to-[#24d3ee] text-white font-semibold py-3 px-8 rounded-lg transform hover:scale-105 transition-all duration-300 hover:shadow-lg hover:shadow-[#ff29f7]/25 active:scale-95">
                CONTINUE TO PATRIK TOMA'S PAGE
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