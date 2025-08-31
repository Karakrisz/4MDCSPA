<script setup>
// Az aktuális események - csak az első 3 megjelenítése a főoldalon
const competitions = [
  { 
    title: 'Grand Opening - First Open Training', 
    location: 'Dubai, UAE', 
    date: 'Fri, 30 Aug',
    type: 'Grand Opening'
  },
  { 
    title: 'GEMS FirstPoint School - Sign-up day', 
    location: 'GEMS FirstPoint School', 
    date: 'Sun, 1 Sept',
    type: 'Sign-up'
  },
  { 
    title: 'Raffles World Academy - First AACA training', 
    location: 'Raffles World Academy', 
    date: 'Sun, 8 Sept',
    type: 'Training'
  }
];

import { ref, onMounted, onUnmounted } from 'vue'

const competitionsSection = ref(null)

// Router navigáció a teljes lista oldalra
const navigateToAllEvents = () => {
  // Itt a router push-t használnád: $router.push('/competitions')
  console.log('Navigate to all competitions page')
}

onMounted(() => {
  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add('animate-in')
        }
      })
    },
    {
      threshold: 0.1,
      rootMargin: '0px 0px -100px 0px'
    }
  )

  // Animálandó elemek megfigyelése
  const elementsToAnimate = competitionsSection.value?.querySelectorAll(
    '.competitions-title, .competitions-box, .competition-item, .show-all-button'
  )

  elementsToAnimate?.forEach((el, index) => {
    // Verseny elemek esetében lépcsőzetes delay
    if (el.classList.contains('competition-item')) {
      el.style.setProperty('--item-delay', `${index * 0.2}s`)
    }
    observer.observe(el)
  })

  onUnmounted(() => {
    observer.disconnect()
  })
})
</script>

<template>
  <section class="relative text-white competitions-section" ref="competitionsSection" id="COMPETITIONS">
    <!-- Background image -->
    <div class="absolute inset-0 bg-container">
      <NuxtImg 
        src="/img/com.webp" 
        alt="Competitions background" 
        class="w-full h-full object-cover bg-image" 
      />
      <div class="absolute inset-0 bg-black opacity-60 bg-overlay"></div>
    </div>

    <div class="relative container mx-auto px-4 py-24 flex flex-col items-center">
      <!-- Title -->
      <h2 class="text-[24px] lg:text-[64px] font-extrabold font-unbounded uppercase mb-5 text-center text-white competitions-title opacity-0">
        Upcoming Events
      </h2>

      <!-- Competitions Box -->
      <div class="bg-white rounded-lg shadow-lg w-full max-w-2xl p-8 competitions-box opacity-0">
        <div 
          v-for="(comp, idx) in competitions" 
          :key="idx" 
          class="flex items-center py-4 border-b last:border-0 competition-item opacity-0"
        >
          <div class="w-20 h-20 bg-gray-200 rounded mr-6 flex-shrink-0 competition-icon">
            <!-- Event type based icon -->
            <div class="w-full h-full rounded bg-gradient-to-br from-gray-300 to-gray-400 flex items-center justify-center icon-content">
              <div v-if="comp.type === 'Grand Opening'" class="w-8 h-8 border-4 border-[#00BCD4] rounded-full animate-pulse"></div>
              <div v-else-if="comp.type === 'Sign-up'" class="w-6 h-6 bg-[#FE2AF7] rounded"></div>
              <div v-else class="w-8 h-8 border-4 border-[#242424] rounded-full animate-pulse"></div>
            </div>
          </div>
          <div class="competition-details">
            <h3 class="text-[20px] font-extrabold text-[#242424] font-unbounded competition-title">
              {{ comp.title }}
            </h3>
            <p class="mt-1 text-[#242424] competition-location">
              {{ comp.location }}
            </p>
            <p class="mt-1 font-semibold text-[#242424] competition-date">
              {{ comp.date }}
            </p>
          </div>
        </div>

        <!-- Show All Button -->
        <div class="mt-8 text-center">
          <NuxtLink to="/all-events"
            class="bg-[#242424] text-white text-[16px] uppercase font-bold py-4 px-20 shadow-lg show-all-button opacity-0 hover:bg-gradient-to-r hover:from-[#00BCD4] hover:to-[#FE2AF7] transition-all duration-300"
          >
            Show All Events
          </NuxtLink>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.competitions-section {
  position: relative;
  min-height: 100vh;
  display: flex;
  align-items: center;
}

/* Háttér animációk */
.bg-container {
  overflow: hidden;
}

.bg-image {
  transform: scale(1.1);
  animation: backgroundDrift 25s ease-in-out infinite;
}

@keyframes backgroundDrift {
  0%, 100% {
    transform: scale(1.1) translateX(0px) translateY(0px);
  }
  25% {
    transform: scale(1.15) translateX(-20px) translateY(-10px);
  }
  50% {
    transform: scale(1.12) translateX(15px) translateY(-5px);
  }
  75% {
    transform: scale(1.18) translateX(-10px) translateY(10px);
  }
}

.bg-overlay {
  animation: overlayBreathe 6s ease-in-out infinite;
}

@keyframes overlayBreathe {
  0%, 100% { opacity: 0.6; }
  50% { opacity: 0.4; }
}

/* Főcím animáció */
.competitions-title {
  transform: translateY(60px) scale(0.8);
  transition: all 1.2s cubic-bezier(0.34, 1.56, 0.64, 1);
  text-shadow: 0 0 30px rgba(255, 255, 255, 0.5);
  background: linear-gradient(45deg, #ffffff, #00BCD4, #FE2AF7, #ffffff);
  background-size: 300% 100%;
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  animation: titleGradient 3s ease-in-out infinite;
}

@keyframes titleGradient {
  0%, 100% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
}

.competitions-title.animate-in {
  opacity: 1;
  transform: translateY(0) scale(1);
  text-shadow: 0 0 50px rgba(255, 255, 255, 0.8);
}

/* Competitions box animáció */
.competitions-box {
  transform: translateY(80px) rotateX(15deg);
  transition: all 1s cubic-bezier(0.25, 0.46, 0.45, 0.94) 0.3s;
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.3);
}

.competitions-box.animate-in {
  opacity: 1;
  transform: translateY(0) rotateX(0deg);
  box-shadow: 0 30px 60px rgba(0, 0, 0, 0.4);
}

/* Verseny elem animációk */
.competition-item {
  transform: translateX(-50px);
  transition: all 0.8s cubic-bezier(0.25, 0.46, 0.45, 0.94) calc(0.6s + var(--item-delay, 0s));
  border-bottom: 1px solid #e2e8f0;
  position: relative;
}

.competition-item.animate-in {
  opacity: 1;
  transform: translateX(0);
}

.competition-item::before {
  content: '';
  position: absolute;
  left: 0;
  bottom: 0;
  width: 0;
  height: 2px;
  background: linear-gradient(90deg, #00BCD4, #FE2AF7);
  transition: width 0.5s ease;
}

.competition-item.animate-in::before {
  width: 100%;
  transition-delay: calc(1.2s + var(--item-delay, 0s));
}

/* Icon animációk */
.competition-icon {
  transform: scale(0) rotate(-180deg);
  transition: all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1) calc(0.8s + var(--item-delay, 0s));
}

.competition-item.animate-in .competition-icon {
  transform: scale(1) rotate(0deg);
}

.icon-content {
  position: relative;
  overflow: hidden;
}

.icon-content::after {
  content: '';
  position: absolute;
  top: -50%;
  left: -50%;
  width: 200%;
  height: 200%;
  background: linear-gradient(45deg, transparent, rgba(0, 188, 212, 0.3), transparent);
  transform: rotate(45deg);
  animation: iconShimmer 2s ease-in-out infinite;
}

@keyframes iconShimmer {
  0% { transform: translateX(-100%) translateY(-100%) rotate(45deg); }
  100% { transform: translateX(100%) translateY(100%) rotate(45deg); }
}

/* Verseny részletek animációi */
.competition-details {
  transform: translateY(20px);
  transition: all 0.6s cubic-bezier(0.25, 0.46, 0.45, 0.94) calc(1s + var(--item-delay, 0s));
}

.competition-item.animate-in .competition-details {
  transform: translateY(0);
}

.competition-title {
  transition: all 0.3s ease;
}

.competition-location {
  transition: all 0.3s ease;
}

.competition-date {
  transition: all 0.3s ease;
  position: relative;
}

.competition-date::after {
  content: '';
  position: absolute;
  right: -10px;
  top: 50%;
  transform: translateY(-50%);
  width: 6px;
  height: 6px;
  background: #FE2AF7;
  border-radius: 50%;
  opacity: 0;
  animation: datePulse 2s ease-in-out infinite;
}

@keyframes datePulse {
  0%, 100% { opacity: 0; transform: translateY(-50%) scale(0.5); }
  50% { opacity: 1; transform: translateY(-50%) scale(1); }
}

/* Hover effektek verseny elemekre */
.competition-item:hover {
  background: linear-gradient(90deg, rgba(0, 188, 212, 0.05), rgba(254, 42, 247, 0.05));
  transform: translateX(10px);
  transition: all 0.3s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}

.competition-item:hover .competition-title {
  color: #00BCD4;
  transform: translateX(5px);
}

.competition-item:hover .competition-icon {
  transform: scale(1.1) rotate(5deg);
  box-shadow: 0 5px 15px rgba(0, 188, 212, 0.3);
}

.competition-item:hover .competition-date::after {
  opacity: 1;
  animation: none;
}

/* Show All button animáció */
.show-all-button {
  transform: translateY(40px) scale(0.8);
  transition: all 1s cubic-bezier(0.34, 1.56, 0.64, 1) 1.2s;
  position: relative;
  overflow: hidden;
}

.show-all-button.animate-in {
  opacity: 1;
  transform: translateY(0) scale(1);
}

.show-all-button::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
  transition: left 0.6s ease;
}

.show-all-button:hover::before {
  left: 100%;
}

.show-all-button:hover {
  transform: translateY(-5px) scale(1.05);
  box-shadow: 0 15px 30px rgba(36, 36, 36, 0.4);
  transition: all 0.3s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}

/* Responsive finomhangolások */
@media (max-width: 1024px) {
  .competitions-title {
    transform: translateY(40px) scale(0.9);
  }
  
  .competitions-box {
    transform: translateY(50px);
  }
}

@media (max-width: 768px) {
  .competitions-title {
    transform: translateY(30px);
    font-size: 32px;
  }
  
  .competitions-title.animate-in {
    transform: translateY(0);
  }
  
  .competition-item {
    transform: translateY(30px);
  }
  
  .competition-item.animate-in {
    transform: translateY(0);
  }
}

/* Accessibility - prefers-reduced-motion */
@media (prefers-reduced-motion: reduce) {
  .bg-image,
  .bg-overlay {
    animation: none;
  }
  
  .competitions-title {
    animation: none;
    background: white;
    -webkit-text-fill-color: white;
  }
  
  .competitions-title,
  .competitions-box,
  .competition-item,
  .show-all-button {
    opacity: 1;
    transform: none;
    transition: none;
  }
  
  .competition-icon {
    transform: scale(1) rotate(0deg);
  }
  
  .competition-details {
    transform: translateY(0);
  }
  
  .icon-content::after {
    display: none;
  }
  
  .competition-date::after {
    display: none;
  }
  
  .show-all-button::before {
    display: none;
  }
}

/* Extra vizuális effektek */
.competitions-box::before {
  content: '';
  position: absolute;
  top: -2px;
  left: -2px;
  right: -2px;
  bottom: -2px;
  background: linear-gradient(45deg, #00BCD4, #FE2AF7, #00BCD4);
  border-radius: 10px;
  z-index: -1;
  opacity: 0;
  transition: opacity 0.3s ease;
  animation: borderGlow 3s ease-in-out infinite;
}

@keyframes borderGlow {
  0%, 100% { opacity: 0; }
  50% { opacity: 0.3; }
}

.competitions-box.animate-in::before {
  opacity: 0.2;
}

.competitions-box:hover::before {
  opacity: 0.5;
}

/* Loading skeleton a verseny elemekhez */
.competition-item:not(.animate-in) .competition-icon {
  background: linear-gradient(90deg, #f0f0f0, #e0e0e0, #f0f0f0);
  background-size: 200% 100%;
  animation: skeletonShimmer 1.5s infinite;
}

@keyframes skeletonShimmer {
  0% { background-position: -200% 0; }
  100% { background-position: 200% 0; }
}

.competition-item.animate-in .competition-icon {
  animation: none;
  background: linear-gradient(to bottom right, #e2e8f0, #cbd5e0);
}
</style>