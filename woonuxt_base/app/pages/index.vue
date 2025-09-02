<template>
  <main>
    <Hero />
    <!-- <Discounts /> -->
    <AboutSection />
    <OurwordSection />
    <GoogleMapSection />
    <TeamSection />
    <CompetitionsSection />

    <!-- Back to Top Button -->
    <div class="fixed bottom-8 right-8 z-50">
      <button
        @click="scrollToTop"
        class="bg-gradient-to-r from-[#FE2AF7] to-[#24d3ee] hover:from-[#e018d4] hover:to-[#1fb8d4] text-white rounded-full p-4 shadow-2xl hover:shadow-[#FE2AF7]/30 transform hover:scale-110 transition-all duration-300 animate-pulse"
        title="Back to Top"
      >
        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 15l7-7 7 7"></path>
        </svg>
      </button>
    </div>
  </main>
</template>

<script setup>
const scrollToTop = () => {
  window.scrollTo({
    top: 0,
    behavior: 'smooth'
  })
}
</script>

<style scoped>
.brand img {
  max-height: min(8vw, 120px);
  object-fit: contain;
  object-position: center;
}

/* Pulse animation for the floating button */
@keyframes pulse {
  0%, 100% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.05);
  }
}

.animate-pulse {
  animation: pulse 2s infinite;
}
</style>