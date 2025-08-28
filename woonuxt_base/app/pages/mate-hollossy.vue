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
  title: 'Máté Hollóssy - Coach | Move Dance Sport Academy',
  meta: [
    {
      name: 'description',
      content:
        "Meet Máté Hollóssy, Coach at Move Dance Sport Academy. World Champion in Acrobatic Rock 'N' Show and Hip-Hop with decades of international experience.",
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
                src="/img/mate.webp"
                alt="Máté Hollóssy"
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
              MÁTÉ
            </h1>
            <h2
              class="text-2xl lg:text-4xl font-bold tracking-wide animate-gradient-text bg-gradient-to-r from-[#ff29f7] to-[#24d3ee] bg-clip-text text-transparent">
              HOLLÓSSY
            </h2>
            <div class="space-y-0.5 mt-3">
              <p class="text-base lg:text-lg text-gray-300 font-light">COACH</p>
            </div>
          </div>

          <!-- Contact Info -->
          <div
            class="bg-gradient-to-r from-[#ff29f7] to-[#24d3ee] p-3 rounded-lg transform hover:scale-105 transition-all duration-300 hover:shadow-lg hover:shadow-[#ff29f7]/25">
            <p class="text-white font-semibold text-base">+971 50 133 8270</p>
            <p class="text-white font-semibold text-sm">mate.hollossy@movedsa.com</p>
          </div>

          <!-- Content Sections -->
          <div class="space-y-6">
            <!-- Professional Journey -->
            <div class="transform transition-all duration-500 hover:translate-x-2">
              <h3 class="text-lg font-bold mb-2 text-[#24d3ee]">Professional Journey</h3>
              <p class="text-gray-300 leading-relaxed text-sm">
                With decades of experience in the dance world, I have built an internationally recognized career in Acrobatic Rock 'N' Show and Hip-Hop genres. My passion for movement captured me early in elementary school when I first encountered the dynamic world of Acrobatic Rock 'N' Show. This passion remains unwavering, and now as a coach, I pass on my knowledge to the next generation.
              </p>
            </div>

            <!-- Competitive Achievements -->
            <div class="transform transition-all duration-500 hover:translate-x-2">
              <h3 class="text-lg font-bold mb-2 text-[#ff29f7]">Competitive Achievements</h3>
              <p class="text-gray-300 leading-relaxed text-sm mb-3">
                I began my career in the Acrobatic Rock 'N' Show couples category, then achieved outstanding international success in Hip-Hop:
              </p>
              <div class="text-gray-300 leading-relaxed text-sm space-y-2">
                <p>• IPDSO Hungarian National Championship title</p>
                <p>• MLTSZ European Championship victory</p>
                <p>• WDA World Championship Adult Team Category victory (2022, 2023)</p>
                <p>• WDA World Championship Adult Duo/Trio Category Silver Medal (2023)</p>
              </div>
            </div>

            <!-- Coaching Philosophy -->
            <div class="transform transition-all duration-500 hover:translate-x-2">
              <h3 class="text-lg font-bold mb-2 text-[#24d3ee]">Coaching Philosophy</h3>
              <p class="text-gray-300 leading-relaxed text-sm mb-3">
                Since 2021, I have been actively involved in youth development, working with age groups from preschool to juniors. In my coaching work, I place special emphasis on personalized development and talent nurturing. My competitive dancers regularly achieve outstanding results in IPDSO competitions:
              </p>
              <div class="text-gray-300 leading-relaxed text-sm space-y-2">
                <p>• Gold and silver certifications in beginner categories</p>
                <p>• Regular podium placements in junior categories</p>
              </div>
            </div>

            <!-- Professional Qualifications -->
            <div class="transform transition-all duration-500 hover:translate-x-2">
              <h3 class="text-lg font-bold mb-2 text-[#ff29f7]">Professional Qualifications</h3>
              <div class="text-gray-300 leading-relaxed text-sm space-y-2">
                <p>• Acrobatic Rock and Roll Professional Coach Certification (Budapest Sport Union)</p>
                <p>• Hip-Hop Instructor Certification (Debrecen)</p>
                <p>• Ongoing: Sports Management, Eszterházy Károly University</p>
              </div>
            </div>

            <!-- Mission -->
            <div class="transform transition-all duration-500 hover:translate-x-2">
              <h3 class="text-lg font-bold mb-2 text-[#24d3ee]">Mission</h3>
              <p class="text-gray-300 leading-relaxed text-sm">
                Professional humility and personalized development are at the core of my coaching philosophy. My goal is to create opportunities for international-level growth for all my competitive dancers while sharing the joy of dance and love of movement.
              </p>
            </div>

            <!-- CTA Button -->
            <div class="pt-4">
              <NuxtLink to="tamas-kuptsulik"
                class="bg-gradient-to-r from-[#ff29f7] to-[#24d3ee] text-white font-semibold py-3 px-8 rounded-lg transform hover:scale-105 transition-all duration-300 hover:shadow-lg hover:shadow-[#ff29f7]/25 active:scale-95">
                CONTINUE TO TAMÁS KUPTSULIK'S PAGE
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