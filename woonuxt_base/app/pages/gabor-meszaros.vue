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
  title: 'Gábor Mészáros - Co-Owner & Coach | Move Dance Sport Academy',
  meta: [
    {
      name: 'description',
      content:
        "Meet Gábor Mészáros, Co-Owner and Coach at Move Dance Sport Academy. Nearly 30 years of competitive experience and professional coaching expertise in Acrobatic Rock and Roll.",
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
                src="/img/gabor.webp"
                alt="Gábor Mészáros"
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
              GÁBOR
            </h1>
            <h2
              class="text-2xl lg:text-4xl font-bold tracking-wide animate-gradient-text bg-gradient-to-r from-[#ff29f7] to-[#24d3ee] bg-clip-text text-transparent">
              MÉSZÁROS
            </h2>
            <div class="space-y-0.5 mt-3">
              <p class="text-base lg:text-lg text-gray-300 font-light">CO-OWNER</p>
              <p class="text-base lg:text-lg text-gray-300 font-light">COACH</p>
            </div>
          </div>

          <!-- Contact Info -->
          <div
            class="bg-gradient-to-r from-[#ff29f7] to-[#24d3ee] p-3 rounded-lg transform hover:scale-105 transition-all duration-300 hover:shadow-lg hover:shadow-[#ff29f7]/25">
            <p class="text-white font-semibold text-base">+971 58 569 0513</p>
            <p class="text-white font-semibold text-sm">gabor.meszaros@movedsa.com</p>
          </div>

          <!-- Content Sections -->
          <div class="space-y-6">
            <!-- Professional Journey -->
            <div class="transform transition-all duration-500 hover:translate-x-2">
              <h3 class="text-lg font-bold mb-2 text-[#24d3ee]">Professional Journey</h3>
              <p class="text-gray-300 leading-relaxed text-sm">
                My Acrobatic Rock and Roll career began in 1995, progressing through every competitive level from Children's category to Main Class Freestyle. I've mastered every aspect of dance and acrobatics, training with industry leaders. My performance experience extends beyond competition, including roles in musicals and major concert productions. For eight years, I competed alongside my dance partner and now wife, Mészárosné Lengyel Bonita. Together, we now coach Acrobatic Rock and Roll couples and teams. Throughout my nearly 30-year competitive career, I have earned multiple regional and national championships, alongside numerous international victories and podium placements.
              </p>
            </div>

            <!-- Coaching Excellence -->
            <div class="transform transition-all duration-500 hover:translate-x-2">
              <h3 class="text-lg font-bold mb-2 text-[#ff29f7]">Coaching Excellence</h3>
              <p class="text-gray-300 leading-relaxed text-sm">
                My coaching career began at age 16, obtaining my professional certification in 2007, renewed in 2021. Recognizing the importance of effective communication in athlete development, I earned a Media and Communications degree from the University of Dunaújváros in 2023.
              </p>
              <p class="text-gray-300 leading-relaxed text-sm mt-3">
                As a coach, I've developed numerous regional and national champions who have advanced to international competition level, regularly qualifying for World Cup finals and achieving near-podium results. My coaching philosophy emphasizes both athletic excellence and personal growth, preparing my dancers for success in both sport and life.
              </p>
            </div>

            <!-- At Move Dance Sport Academy Dubai -->
            <div class="transform transition-all duration-500 hover:translate-x-2">
              <h3 class="text-lg font-bold mb-2 text-[#24d3ee]">At Move Dance Sport Academy Dubai</h3>
              <p class="text-gray-300 leading-relaxed text-sm">
                Together with my wife, Bonita Lengyel Mészárosné - we now train hundreds of young athletes at Move Dance Sport Academy Dubai. Our mission is to develop dedicated dancers who excel both in competition and character. We strive to create elite athletes capable of competing at the world's most prestigious dance sport events.
              </p>
              <p class="text-gray-300 leading-relaxed text-sm mt-3">
                Our proven track record in competitive dance sport, combined with our comprehensive coaching methodology, provides a unique foundation for developing the next generation of champions in the United Arab Emirates.
              </p>
            </div>

            <!-- CTA Button -->
            <div class="pt-4">
              <NuxtLink to="bonita-lengyel"
                class="bg-gradient-to-r from-[#ff29f7] to-[#24d3ee] text-white font-semibold py-3 px-8 rounded-lg transform hover:scale-105 transition-all duration-300 hover:shadow-lg hover:shadow-[#ff29f7]/25 active:scale-95">
                CONTINUE TO BONITA LENGYEL MÉSZÁROSNÉ'S PAGE
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