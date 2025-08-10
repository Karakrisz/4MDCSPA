<script setup>
const champions = [
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2012' },
  { category: 'Soldiers Junior Couple Formation', names: '', year: '2013' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2013' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2014' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2014' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2015' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2016' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2018' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2019' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2019' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2019' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2019' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2021' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2021' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2021' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2021' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2022' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2022' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2022' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2022' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2022' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2022' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2023' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2023' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2022' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2022' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2023' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2023' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2024' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2024' },
];

import { ref, onMounted, onUnmounted } from 'vue'

const championsSection = ref(null)

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
      rootMargin: '0px 0px -50px 0px'
    }
  )

  // Animálandó elemek megfigyelése
  const elementsToAnimate = championsSection.value?.querySelectorAll(
    '.champions-heading, .champions-description, .champion-card, .cta-heading, .cta-button'
  )

  elementsToAnimate?.forEach((el, index) => {
    // Kártyák esetében lépcsőzetes delay
    if (el.classList.contains('champion-card')) {
      el.style.setProperty('--card-delay', `${(index % 8) * 0.1}s`)
    }
    observer.observe(el)
  })

  onUnmounted(() => {
    observer.disconnect()
  })
})
</script>

<template>
  <section class="relative text-white pt-16" ref="championsSection">
    <!-- Background Image & Overlay -->
    <div class="absolute inset-0 bg-container">
      <NuxtImg src="/img/word.webp" alt="Champions background" class="w-full h-full object-cover bg-image" />
      <div class="absolute inset-0 bg-black opacity-75 overlay-1"></div>
    </div>
    <div class="absolute inset-0 overlay-2" style="opacity: 0.6; background: #141414;"></div>

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
          :key="index" 
          class="bg-white text-[#242424] rounded-lg shadow-lg flex flex-col pt-8 px-8 min-h-[200px] relative champion-card opacity-0" 
          :class="{ 'lg:col-start-2': index === champions.length - 2, 'lg:col-start-3': index === champions.length - 1 }"
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
    </div>

    <!-- Training Programs CTA Container -->
    <div class="relative mt-16 cta-section" style="background: var(--Dark, #242424)">
      <div class="container mx-auto px-4 py-12">
        <div class="text-center">
          <h3 class="font-unbounded text-[30px] font-extrabold uppercase text-white cta-heading opacity-0">
            Explore our<br />Training Programs
          </h3>
          <div class="inline-flex items-center mt-6">
            <button class="bg-[#FE2AF7] text-[16px] text-black uppercase font-bold py-4 px-20 shadow-lg cta-button opacity-0">
              Coming Soon
            </button>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
/* Háttér animációk */
.bg-container {
  overflow: hidden;
}

.bg-image {
  transform: scale(1.1);
  animation: backgroundPan 30s ease-in-out infinite;
}

@keyframes backgroundPan {
  0%, 100% {
    transform: scale(1.1) translateX(0px);
  }
  50% {
    transform: scale(1.15) translateX(-20px);
  }
}

.overlay-1 {
  animation: overlayPulse 4s ease-in-out infinite;
}

@keyframes overlayPulse {
  0%, 100% { opacity: 0.75; }
  50% { opacity: 0.65; }
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
  box-shadow: 0 20px 40px rgba(254, 42, 247, 0.3);
  transition: all 0.4s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}

.champion-card:hover .card-year {
  color: #FE2AF7;
  text-shadow: 0 0 20px rgba(254, 42, 247, 0.6);
  transition: all 0.3s ease;
}

.champion-card:hover .card-title {
  color: #00BCD4;
  transition: all 0.3s ease;
}

/* Kártya tartalom animációk */
.card-title {
  transition: all 0.3s ease;
}

.card-names {
  transition: all 0.3s ease;
}

.card-year {
  transition: all 0.3s ease;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
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

/* Grid responsive animációk */
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
  .cta-button {
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
  background: linear-gradient(90deg, #f0f0f0, #e0e0e0, #f0f0f0);
  background-size: 200% 100%;
  animation: shimmerLoading 1.5s infinite;
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
  background: white;
  animation: none;
}
</style>