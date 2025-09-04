<script setup>
const allChampions = [
  // 2012
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2012' },
  
  // 2013  
  { category: 'Junior Couple Formation', names: 'Soldiers', year: '2013' },
  { category: 'Junior Girls Formation', names: 'Party Girls', year: '2013' },
  
  // 2014
  { category: 'Junior Couple', names: 'Ádám Király - Fanni Zetl', year: '2014' },
  { category: 'Junior Girls Formation', names: 'Party Girls', year: '2014' },
  { category: 'Junior Girls Formation', names: 'Party Girls', year: '2015' },
  
  // 2016
  { category: 'Ladies Formation', names: 'Szupergirls', year: '2016' },
  { category: 'Ladies Formation', names: 'The Szupergirls', year: '2018' },
  { category: 'Junior Girls Formation', names: 'Party Team', year: '2018' },
  
  // 2019
  { category: 'Junior Couple Formation', names: 'Home Alone', year: '2019' },
  { category: 'Junior Girls Formation', names: 'Party Team', year: '2019' },
  { category: 'Ladies Formation', names: 'The Szupergirls', year: '2019' },
  
  // 2021
  { category: 'Ladies Formation', names: 'The Szupergirls', year: '2021' },
  { category: 'Junior Couple Formation', names: 'Home Alone', year: '2021' },
  { category: 'Junior Couple', names: 'Attila Ancsák - Júlia Ökrös', year: '2021' },
  
  // 2021 continued
  { category: 'Junior Girls Formation', names: 'Party Girls', year: '2021' },
  { category: 'Junior Couple Formation', names: 'Soldiers', year: '2022' },
  { category: 'Main Class Contact Style', names: 'Ádám Király - Greta Mercz', year: '2022' },
  
  // 2022
  { category: 'Main Class Formation', names: 'The Clue', year: '2022' },
  { category: 'Junior Couple', names: 'Attila Ancsák - Júlia Ökrös', year: '2022' },
  { category: 'Junior Girls Formation', names: 'Party Girls', year: '2022' },
  
  // 2022 continued
  { category: 'Ladies Formation', names: 'Origo', year: '2022' },
  { category: 'Main Class Free Style', names: 'Alex Deli - Luca Bihari', year: '2023' },
  { category: 'Junior Couple Formation', names: 'Soldiers', year: '2023' },
  
  // 2024
  { category: 'Junior Couple Formation', names: 'Rock It!', year: '2024' },
  { category: 'Ladies Formation', names: 'The Szupergirls', year: '2024' },
];

import { ref, onMounted, onUnmounted, nextTick, computed } from 'vue'

const championsSection = ref(null)
const isMobile = ref(false)
const mobileVisibleCount = ref(4)
const isLoadingMore = ref(false)

// Mobil eszköz detektálása
const detectMobile = () => {
  return /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent) ||
    window.innerWidth <= 768;
};

// Bajnokok kiválasztása eszköz típus szerint
const champions = computed(() => {
  if (isMobile.value) {
    return allChampions.slice(0, mobileVisibleCount.value);
  }
  return allChampions;
});

// Van-e még több bajnok mobilon
const hasMoreChampions = computed(() => {
  return isMobile.value && mobileVisibleCount.value < allChampions.length;
});

// Még hány bajnok van hátra
const remainingChampions = computed(() => {
  return allChampions.length - mobileVisibleCount.value;
});

// Load More funkció
const loadMoreChampions = async () => {
  if (isLoadingMore.value) return;
  
  isLoadingMore.value = true;
  
  // Animált loading
  await new Promise(resolve => setTimeout(resolve, 300));
  
  const nextBatch = Math.min(4, allChampions.length - mobileVisibleCount.value);
  mobileVisibleCount.value += nextBatch;
  
  isLoadingMore.value = false;
  
  // Újra animáljuk az új kártyákat
  await nextTick();
  setupAnimations();
};

// Resize handler függvény
const handleResize = () => {
  const wasMobile = isMobile.value;
  isMobile.value = detectMobile();
  
  // Ha desktop-ra váltunk, mutassuk az összeset
  if (wasMobile && !isMobile.value) {
    mobileVisibleCount.value = allChampions.length;
  }
  // Ha mobilra váltunk, kezdjük elölről
  else if (!wasMobile && isMobile.value) {
    mobileVisibleCount.value = 4;
  }
  
  // Ha változott a mobil/desktop állapot, újra animálunk
  if (wasMobile !== isMobile.value) {
    setTimeout(() => {
      setupAnimations();
    }, 100);
  }
};

const setupAnimations = () => {
  // Intersection Observer a kártyák animációjához
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
      rootMargin: '0px 0px -50px 0px'
    }
  )

  // Animálandó elemek megfigyelése
  const elementsToAnimate = championsSection.value?.querySelectorAll(
    '.champions-heading, .champions-description, .champion-card, .cta-heading, .cta-button, .load-more-button'
  )

  elementsToAnimate?.forEach((el, index) => {
    // Kártyák esetében lépcsőzetes delay
    if (el.classList.contains('champion-card')) {
      el.style.setProperty('--card-delay', `${(index % 8) * 0.1}s`)
    }
    // Eltávolítjuk az esetleges korábbi animálás osztályt
    el.classList.remove('animate-in');
    observer.observe(el)
  })
};

onMounted(async () => {
  await nextTick();
  
  // Mobil detektálás
  isMobile.value = detectMobile();
  
  // Resize listener hozzáadása
  window.addEventListener('resize', handleResize);

  setupAnimations();
});

// onUnmounted hook közvetlenül a setup() függvény tetején
onUnmounted(() => {
  window.removeEventListener('resize', handleResize);
});
</script>

<template>
  <section class="relative text-white pt-16" ref="championsSection" id="champions">
    <!-- Background Image & Overlay -->
    <div class="absolute inset-0 bg-container">
      <!-- Háttérkép -->
      <NuxtImg 
        src="/img/word.webp" 
        alt="Champions background" 
        class="w-full h-full object-cover bg-image" 
      />
      
      <div class="absolute inset-0 bg-black opacity-40 overlay-1"></div>
    </div>
    <div class="absolute inset-0 overlay-2" style="opacity: 0.3; background: #141414;"></div>

    <!-- Main Content -->
    <div class="relative container mx-auto px-4">
      <!-- Heading -->
      <div class="text-center mb-12">
        <h2 class="font-unbounded uppercase text-cyan-400 font-medium text-[20px] lg:text-[32px] champions-heading opacity-0">
          Our World Champions
        </h2>
        <p class="mt-4 max-w-2xl mx-auto text-lg leading-relaxed text-[16px] champions-description opacity-0">
          Our coaches are living legends of the sport! Their competitive dancers have claimed 26 World Championship titles in the past 13 years under the WRRC
          (World Rock'n'Roll Confederation) – an unprecedented achievement in the history of acrobatic rock 'n' roll!
        </p>
      </div>

      <!-- Cards Grid -->
      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
        <div 
          v-for="(item, index) in champions" 
          :key="`${item.year}-${index}`" 
          class="bg-white/20 backdrop-blur-sm border border-white/30 text-white rounded-lg shadow-lg flex flex-col pt-8 px-8 min-h-[200px] relative champion-card opacity-0" 
          :class="{ 
            'lg:col-start-2': !isMobile && index === champions.length - 2, 
            'lg:col-start-3': !isMobile && index === champions.length - 1 
          }"
        >
          <div class="text-center flex-grow">
            <h3 class="text-[18px] lg:text-[18px] uppercase font-bold card-title">
              {{ item.category }}
            </h3>
            <p class="mt-2 text-[16px] lg:text-[16px] font-normal uppercase card-names">
              {{ item.names }}
            </p>
          </div>
          <div class="absolute bottom-[-25px] lg:bottom-[-25px] left-0 right-0 text-center pb-0">
            <span class="text-[65px] lg:text-[65px] font-black font-unbounded card-year">
              {{ item.year }}
            </span>
          </div>
          <!-- Hover overlay -->
          <div class="card-hover-overlay"></div>
        </div>
      </div>

      <!-- Mobile Load More Button -->
      <div v-if="hasMoreChampions" class="text-center mt-12">
        <button 
          @click="loadMoreChampions"
          :disabled="isLoadingMore"
          class="load-more-button opacity-0 relative overflow-hidden bg-gradient-to-r from-cyan-400 to-[#FE2AF7] text-black font-bold text-[16px] uppercase px-8 py-4 rounded-lg shadow-lg transition-all duration-300 hover:shadow-2xl hover:shadow-cyan-400/30 disabled:opacity-70 disabled:cursor-not-allowed group"
        >
          <div class="relative z-10 flex items-center justify-center space-x-2">
            <span v-if="!isLoadingMore">
              Show More Champions
            </span>
            <span v-else class="flex items-center space-x-2">
              <svg class="animate-spin h-5 w-5" fill="none" viewBox="0 0 24 24">
                <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                <path class="opacity-75" fill="currentColor" d="m4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
              </svg>
              <span>Loading...</span>
            </span>
            <div v-if="!isLoadingMore" class="bg-black/20 rounded-full px-3 py-1 text-sm">
              +{{ Math.min(4, remainingChampions) }}
            </div>
          </div>
          
          <!-- Button shine effect -->
          <div class="absolute inset-0 bg-gradient-to-r from-transparent via-white/20 to-transparent -translate-x-full group-hover:translate-x-full transition-transform duration-700 ease-in-out"></div>
        </button>
        
        <p class="text-white/60 text-sm mt-3">
          Showing {{ champions.length }} of {{ allChampions.length }} champions
        </p>
      </div>
    </div>

    <!-- Training Programs CTA Container -->
    <div class="relative mt-16 cta-section" style="background: var(--Dark, #242424)">
      <div class="container mx-auto px-4 py-12">
        <div class="text-center">
          <h3 class="font-unbounded text-[30px] font-extrabold uppercase text-white cta-heading opacity-0">
            Explore our<br />Training Programs
          </h3>
          <div class="inline-flex items-center mt-6">
            <!-- <button class="bg-[#FE2AF7] text-[16px] text-black uppercase font-bold py-4 px-20 shadow-lg cta-button opacity-0">
              Coming Soon
            </button> -->
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
/* [A CSS stílusok változatlanok maradnak] */
/* Háttér animációk */
.bg-container {
  overflow: hidden;
}

.bg-image {
  animation: backgroundFloat 20s ease-in-out infinite;
}

@keyframes backgroundFloat {
  0%, 100% {
    transform: scale(1) translateY(0px);
  }
  50% {
    transform: scale(1.05) translateY(-10px);
  }
}

.overlay-1 {
  animation: overlayPulse 4s ease-in-out infinite;
}

@keyframes overlayPulse {
  0%, 100% { opacity: 0.40; }
  50% { opacity: 0.30; }
}

/* Heading animációk */
.champions-heading {
  transform: translateY(50px) scale(0.9);
  transition: all 1s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}

.champions-heading.animate-in {
  opacity: 1;
  transform: translateY(0) scale(1);
}

.champions-description {
  transform: translateY(30px);
  transition: all 0.8s cubic-bezier(0.25, 0.46, 0.45, 0.94) 0.3s;
}

.champions-description.animate-in {
  opacity: 1;
  transform: translateY(0);
}

/* Kártya animációk */
.champion-card {
  transform: translateY(60px) rotateX(15deg);
  transition: all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1) calc(0.5s + var(--card-delay, 0s));
  position: relative;
  overflow: hidden;
  cursor: pointer;
}

.champion-card.animate-in {
  opacity: 1;
  transform: translateY(0) rotateX(0deg);
}

/* Load More Button animációk */
.load-more-button {
  transform: translateY(20px);
  opacity: 1;
  transition: all 0.6s cubic-bezier(0.34, 1.56, 0.64, 1);
  animation: fadeInUp 0.8s ease-out forwards;
}

@keyframes fadeInUp {
  0% {
    opacity: 0;
    transform: translateY(40px) scale(0.9);
  }
  100% {
    opacity: 1;
    transform: translateY(0) scale(1);
  }
}

.load-more-button.animate-in {
  opacity: 1;
  transform: translateY(0) scale(1);
}

.load-more-button:hover {
  transform: translateY(-3px) scale(1.05);
  box-shadow: 0 20px 40px rgba(0, 188, 212, 0.3), 0 20px 40px rgba(254, 42, 247, 0.2);
}

.load-more-button:active {
  transform: translateY(-1px) scale(1.02);
}

/* Kártya hover overlay */
.card-hover-overlay {
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(254, 42, 247, 0.1), transparent);
  transition: left 0.6s ease;
  pointer-events: none;
}

.champion-card:hover .card-hover-overlay {
  left: 100%;
}

/* Kártya hover effektek */
.champion-card:hover {
  transform: translateY(-10px) scale(1.02);
  box-shadow: 0 20px 40px rgba(254, 42, 247, 0.4);
  background: rgba(255, 255, 255, 0.35);
  backdrop-filter: blur(12px);
  border-color: rgba(254, 42, 247, 0.6);
  transition: all 0.4s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}

.champion-card:hover .card-year {
  color: #FE2AF7;
  text-shadow: 0 0 20px rgba(254, 42, 247, 0.8);
  transition: all 0.3s ease;
}

.champion-card:hover .card-title {
  color: #00BCD4;
  text-shadow: 0 2px 8px rgba(0, 188, 212, 0.6);
  transition: all 0.3s ease;
}

.champion-card:hover .card-names {
  color: rgba(255, 255, 255, 0.95);
  text-shadow: 0 1px 4px rgba(0, 0, 0, 0.8);
  transition: all 0.3s ease;
}

/* Kártya tartalom animációk */
.card-title {
  transition: all 0.3s ease;
  text-shadow: 0 1px 3px rgba(0, 0, 0, 0.7);
}

.card-names {
  transition: all 0.3s ease;
  text-shadow: 0 1px 3px rgba(0, 0, 0, 0.6);
}

.card-year {
  transition: all 0.3s ease;
  text-shadow: 2px 2px 8px rgba(0, 0, 0, 0.8);
  color: rgba(255, 255, 255, 0.9);
}

/* CTA szekció animációk */
.cta-heading {
  transform: translateY(40px);
  transition: all 0.8s cubic-bezier(0.25, 0.46, 0.45, 0.94) 0.2s;
}

.cta-heading.animate-in {
  opacity: 1;
  transform: translateY(0);
}

.cta-button {
  transform: translateY(30px) scale(0.9);
  transition: all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1) 0.4s;
  position: relative;
  overflow: hidden;
}

.cta-button.animate-in {
  opacity: 1;
  transform: translateY(0) scale(1);
}

/* CTA button hover effektek */
.cta-button:hover {
  transform: translateY(-3px) scale(1.05);
  box-shadow: 0 10px 30px rgba(254, 42, 247, 0.4);
  background: linear-gradient(45deg, #FE2AF7, #FF6B9D);
  transition: all 0.3s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}

.cta-button::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
  transition: left 0.6s ease;
}

.cta-button:hover::before {
  left: 100%;
}

/* Mobil optimalizálások */
@media (max-width: 1024px) {
  .champion-card {
    transform: translateY(40px);
  }
  
  .champion-card.animate-in {
    transform: translateY(0);
  }
}

@media (max-width: 640px) {
  .champion-card {
    transform: translateY(30px) scale(0.95);
  }
  
  .champion-card.animate-in {
    transform: translateY(0) scale(1);
  }
  
  .load-more-button {
    font-size: 14px;
    px: 6;
    py: 3;
  }
}

/* Desktop esetén elrejtjük a load more gombot */
@media (min-width: 769px) {
  .load-more-button {
    display: none !important;
  }
}

/* Accessibility - prefers-reduced-motion */
@media (prefers-reduced-motion: reduce) {
  .bg-image,
  .overlay-1 {
    animation: none;
  }
  
  .champions-heading,
  .champions-description,
  .champion-card,
  .cta-heading,
  .cta-button,
  .load-more-button {
    opacity: 1;
    transform: none;
    transition: none;
  }
  
  .card-hover-overlay {
    display: none;
  }
  
  .cta-button::before {
    display: none;
  }
}

/* Extra vizuális effektek */
.champions-heading {
  text-shadow: 0 0 30px rgba(0, 188, 212, 0.5);
}

.champions-heading.animate-in {
  text-shadow: 0 0 40px rgba(0, 188, 212, 0.7);
}

/* Kártya border glow */
.champion-card::after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  border-radius: 8px;
  padding: 2px;
  background: linear-gradient(45deg, transparent, rgba(254, 42, 247, 0.3), transparent);
  mask: linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0);
  mask-composite: exclude;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.champion-card:hover::after {
  opacity: 1;
}

/* Loading state animáció */
.champion-card:not(.animate-in) {
  background: linear-gradient(90deg, rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0.2), rgba(255, 255, 255, 0.1));
  background-size: 200% 100%;
  animation: shimmerLoading 1.5s infinite;
  backdrop-filter: blur(4px);
}

@keyframes shimmerLoading {
  0% {
    background-position: -200% 0;
  }
  100% {
    background-position: 200% 0;
  }
}

.champion-card.animate-in {
  background: rgba(255, 255, 255, 0.2);
  backdrop-filter: blur(8px);
  animation: none;
}
</style>