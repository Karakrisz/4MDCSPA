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
  title: 'Veronika Lengyel - Academy Co-Owner & Coach | Move Dance Sport Academy',
  meta: [
    {
      name: 'description',
      content:
        "Meet Veronika Lengyel, Academy Co-Owner and Coach at Move Dance Sport Academy. World Champion coach with 24 World Championship and 11 European Championship titles.",
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
                src="/img/veronika.webp"
                alt="Veronika Lengyel"
                class="w-full h-[600px] lg:h-[1500px] object-cover object-top transform hover:scale-105 transition-transform duration-500 pt-[50px] lg:pt-0" />
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
              VERONIKA
            </h1>
            <h2
              class="text-2xl lg:text-4xl font-bold tracking-wide animate-gradient-text bg-gradient-to-r from-[#ff29f7] to-[#24d3ee] bg-clip-text text-transparent">
              LENGYEL
            </h2>
            <div class="space-y-0.5 mt-3">
              <p class="text-base lg:text-lg text-gray-300 font-light">ACADEMY CO-OWNER</p>
              <p class="text-base lg:text-lg text-gray-300 font-light">COACH</p>
            </div>
          </div>

          <!-- Contact Info -->
          <div
            class="bg-gradient-to-r from-[#ff29f7] to-[#24d3ee] p-3 rounded-lg transform hover:scale-105 transition-all duration-300 hover:shadow-lg hover:shadow-[#ff29f7]/25">
            <p class="text-white font-semibold text-base">+36 70 378 5473</p>
            <p class="text-white font-semibold text-sm">info@movedsa.com</p>
          </div>

          <!-- Content Sections -->
          <div class="space-y-6">
            <!-- Professional Journey -->
            <div class="transform transition-all duration-500 hover:translate-x-2">
              <h3 class="text-lg font-bold mb-2 text-[#24d3ee]">Professional Journey</h3>
              <p class="text-gray-300 leading-relaxed text-sm">
                I have been coaching Acrobatic Rock and Roll since 2003. In 2012, I founded the now world-renowned Rock and Magic Sports Club. As a competitor, I achieved multiple Hungarian National Championships and World Cup victories in junior and adult B categories. Over the years, I have developed numerous exceptional talents who now represent the international elite.
              </p>
            </div>

            <!-- Qualifications and Awards -->
            <div class="transform transition-all duration-500 hover:translate-x-2">
              <h3 class="text-lg font-bold mb-2 text-[#ff29f7]">Qualifications and Awards</h3>
              <div class="text-gray-300 leading-relaxed text-sm space-y-2">
                <p>• Dance Pedagogy Degree in Fashion and Ballroom Dance, Hungarian Dance Academy</p>
                <p>• Professional Coach Certification in Acrobatic Rock and Roll</p>
                <p>• International Judge Certification (since 2000)</p>
                <p>• Competition Supervisor License (since 2010)</p>
                <p>• Five-time "Coach of the Year" Award (2012-2017)</p>
                <p>• Five-time Excellence Award (2018, 2019, 2020, 2023, 2025)</p>
              </div>
            </div>

            <!-- International Success -->
            <div class="transform transition-all duration-500 hover:translate-x-2">
              <h3 class="text-lg font-bold mb-2 text-[#24d3ee]">International Success and World Records</h3>
              <p class="text-gray-300 leading-relaxed text-sm mb-3">
                24 World Championship and 11 European Championship titles.
              </p>
              
              <div class="text-gray-300 leading-relaxed text-sm space-y-3">
                <div>
                  <p class="font-semibold text-[#ff29f7]">World Championship titles:</p>
                  <p>• 6 Adult Women's Formation (1x Ricsajok, 5x Szupergirls)</p>
                  <p>• 8 Junior Girls Formation (2x Szupergirls, 5x Party Girls, 2x Party Team)</p>
                  <p>• 4 Junior Couple Formation (3x Soldiers, 2x Home Alone)</p>
                  <p>• 2 Children's Couple and 2 Junior Couple categories</p>
                </div>
                
                <div>
                  <p class="font-semibold text-[#ff29f7]">European Championship titles:</p>
                  <p>• 5 Adult Women's Formation (Szupergirls)</p>
                  <p>• 4 Junior Girls Formation (1x Szupergirls, 2x Party Team, 1x Party Girls)</p>
                  <p>• 1 Junior Couple Formation (Back to the Future)</p>
                  <p>• 1 Junior Couple category</p>
                </div>
              </div>
            </div>

            <!-- World Records -->
            <div class="transform transition-all duration-500 hover:translate-x-2">
              <h3 class="text-lg font-bold mb-2 text-[#ff29f7]">World Records</h3>
              <div class="text-gray-300 leading-relaxed text-sm space-y-2">
                <p>• 14-time Girls Formation World Champion Coach (unmatched achievement)</p>
                <p>• Only coach whose team won all three world competitions in one year, achieved three times (2018, 2019, 2022)</p>
                <p>• Szupergirls: 5-time World and European Champion in 2024 (absolute record)</p>
              </div>
            </div>

            <!-- At Move Dance Sport Academy Dubai -->
            <div class="transform transition-all duration-500 hover:translate-x-2">
              <h3 class="text-lg font-bold mb-2 text-[#24d3ee]">At Move Dance Sport Academy Dubai</h3>
              <p class="text-gray-300 leading-relaxed text-sm">
                I now bring my internationally recognized methodology and two decades of experience to Dubai. My coaching philosophy combines professional development with fostering a love for dance. My goal is to establish Move Dance Sport Academy as the UAE's leading dance academy and develop the next generation of champions.
              </p>
              <p class="text-gray-300 leading-relaxed text-sm mt-3">
                My methodology is distinguished by comprehensive training and personalized development. My competitive dancers excel not only as performers but as well-rounded, successful athletes who compete at the highest international level.
              </p>
            </div>

            <!-- CTA Button -->
            <div class="pt-4">
              <NuxtLink to="gabor-meszaros"
                class="bg-gradient-to-r from-[#ff29f7] to-[#24d3ee] text-white font-semibold text-[9px] lg:text-[16px] py-3 px-8 rounded-lg transform hover:scale-105 transition-all duration-300 hover:shadow-lg hover:shadow-[#ff29f7]/25 active:scale-95">
                CONTINUE TO GÁBOR MÉSZÁROS' PAGE
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