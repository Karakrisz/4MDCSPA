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
  title: 'Patrik Toma - Coach | Move Dance Sport Academy',
  meta: [
    {
      name: 'description',
      content:
        "Meet Patrik Toma, Coach at Move Dance Sport Academy. University graduate with coaching degree, World Champion and World Cup medalist in Acrobatic Rock and Roll.",
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
                src="/img/patrik.webp"
                alt="Patrik Toma"
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
              PATRIK
            </h1>
            <h2
              class="text-2xl lg:text-4xl font-bold tracking-wide animate-gradient-text bg-gradient-to-r from-[#ff29f7] to-[#24d3ee] bg-clip-text text-transparent">
              TOMA
            </h2>
            <div class="space-y-0.5 mt-3">
              <p class="text-base lg:text-lg text-gray-300 font-light">COACH</p>
            </div>
          </div>

          <!-- Contact Info -->
          <div
            class="bg-gradient-to-r from-[#ff29f7] to-[#24d3ee] p-3 rounded-lg transform hover:scale-105 transition-all duration-300 hover:shadow-lg hover:shadow-[#ff29f7]/25">
            <p class="text-white font-semibold text-base">+971 50 124 2540</p>
            <p class="text-white font-semibold text-sm">patrik.toma@movedsa.com</p>
          </div>

          <!-- Content Sections -->
          <div class="space-y-6">
            <!-- Professional Journey -->
            <div class="transform transition-all duration-500 hover:translate-x-2">
              <h3 class="text-lg font-bold mb-2 text-[#24d3ee]">Professional Journey</h3>
              <p class="text-gray-300 leading-relaxed text-sm">
                I graduated from the University of Physical Education (TF) in 2025 with a coaching degree. From competitive dancer to coach, I combine practical experience with theoretical knowledge.
              </p>
            </div>

            <!-- Competitive Achievements -->
            <div class="transform transition-all duration-500 hover:translate-x-2">
              <h3 class="text-lg font-bold mb-2 text-[#ff29f7]">Competitive Achievements</h3>
              <div class="text-gray-300 leading-relaxed text-sm space-y-2">
                <p>• National championships in youth and junior categories</p>
                <p>• World Cup victories and silver medals</p>
                <p>• European and World Championship titles in junior couple formations</p>
              </div>
            </div>

            <!-- Coaching Success -->
            <div class="transform transition-all duration-500 hover:translate-x-2">
              <h3 class="text-lg font-bold mb-2 text-[#24d3ee]">Coaching Success</h3>
              <p class="text-gray-300 leading-relaxed text-sm">
                During my four-year coaching career, I've trained couples and formations. My greatest achievement is a World Cup silver medal with my students in couple formation, second only to our academy's "A" team.
              </p>
            </div>

            <!-- At Move Dance Sport Academy Dubai -->
            <div class="transform transition-all duration-500 hover:translate-x-2">
              <h3 class="text-lg font-bold mb-2 text-[#ff29f7]">At Move Dance Sport Academy Dubai</h3>
              <p class="text-gray-300 leading-relaxed text-sm">
                I integrate anatomical and psychological knowledge from my university studies with practical experience in training my competitors. My coaching philosophy centers on teamwork, perseverance, and determination. My goal is to prepare dancers for competition success both technically and mentally.
              </p>
            </div>

            <!-- CTA Button -->
            <div class="pt-4">
              <NuxtLink to="mate-hollossy"
                class="bg-gradient-to-r from-[#ff29f7] to-[#24d3ee] text-white font-semibold text-[9px] lg:text-[16px] py-3 px-8 rounded-lg transform hover:scale-105 transition-all duration-300 hover:shadow-lg hover:shadow-[#ff29f7]/25 active:scale-95">
                CONTINUE TO MÁTÉ HOLLÓSSY'S PAGE
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