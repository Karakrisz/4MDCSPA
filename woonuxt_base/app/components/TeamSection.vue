<script setup>
const coaches = [
  { name: 'Tamás Kuptsulik', role: 'Academy Director / Coach', photo: '/img/tamas.webp' },
  { name: 'Veronika Lengyel', role: 'Academy Co-owner / Coach', photo: '/img/veronika.webp' },
  { name: 'Gábor Mészáros', role: 'Academy Co-owner / Coach', photo: '/img/gabor.webp' },
  { name: 'Bonita Lengyel Mészárosné', role: 'Coach', photo: '/img/boni.webp' },
  { name: 'Patrik Toma', role: 'Coach', photo: '/img/patrik.webp' },
  { name: 'Máté Hollóssy', role: 'Coach', photo: '/img/mate.webp' },
];

import { ref, onMounted, onUnmounted } from 'vue'

const coachingSection = ref(null)

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
  const elementsToAnimate = coachingSection.value?.querySelectorAll(
    '.coaching-heading, .coaching-description, .coach-card, .cta-section-heading, .cta-section-description, .cta-button'
  )

  elementsToAnimate?.forEach((el, index) => {
    // Coach kártyák esetében lépcsőzetes delay
    if (el.classList.contains('coach-card')) {
      el.style.setProperty('--coach-delay', `${(index % 6) * 0.15}s`)
    }
    observer.observe(el)
  })

  onUnmounted(() => {
    observer.disconnect()
  })
})
</script>

<template>
  <section class="relative text-white pt-16 coaching-section" ref="coachingSection">
    <!-- Gradient Background -->
    <div class="absolute inset-0 mix-blend-multiply gradient-bg" style="background: linear-gradient(180deg, rgba(0, 0, 0, 0) 55.37%, #000 100%)"></div>

    <div class="relative container mx-auto pt-4 px-4">
      <!-- Heading (left aligned) -->
      <div class="mb-12 text-left max-w-3xl">
        <h2 class="font-unbounded uppercase text-cyan-400 font-medium text-[20px] lg:text-[32px] coaching-heading opacity-0">
          Our Elite <br> Coaching Team
        </h2>
        <p class="mt-4 text-lg leading-relaxed text-[16px] coaching-description opacity-0">
          Our coaches are living legends of the sport! Their competitive dancers have claimed 26 World Championship titles in the past 13 years under the WRRC
          (World Rock'n'Roll Confederation) – an unprecedented achievement in the history of acrobatic rock 'n' roll!
        </p>
      </div>

      <!-- Team Grid with larger gap and taller images -->
      <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-6 gap-1">
        <div 
          v-for="(coach, i) in coaches" 
          :key="i" 
          class="relative overflow-hidden coach-card opacity-0"
        >
          <!-- Image with color overlay and fixed height -->
          <div class="relative image-container">
            <NuxtImg 
              :src="coach.photo" 
              :alt="coach.name" 
              class="w-full sm:h-full lg:h-[450px] object-cover coach-image" 
            />
            <div class="absolute inset-0 bg-cyan-400 opacity-100 mix-blend-multiply color-overlay"></div>
            <!-- Hover overlay -->
            <div class="image-hover-overlay"></div>
          </div>
          
          <!-- Text block -->
          <div class="absolute bottom-0 left-0 w-full p-4 bg-gradient-to-t from-black to-transparent text-overlay">
            <h3 class="text-[20px] font-unbounded font-medium uppercase coach-name">
              {{ coach.name }}
            </h3>
            <p class="text-[14px] uppercase mt-1 coach-role">
              {{ coach.role }}
            </p>
          </div>
        </div>
      </div>
    </div>
    
    <!-- Event Performance CTA Container -->
    <div class="relative bg-[#242424] cta-section">
      <div class="container mx-auto px-4 py-12">
        <div class="text-center">
          <h3 class="text-[20px] lg:text-[20px] font-unbounded uppercase font-bold cta-section-heading opacity-0">
            Elevate your event with a breathtaking <br> acrobatic Rock 'n' Show performance!
          </h3>
          <p class="mt-2 text-[16px] lg:text-[16px] font-unbounded font-normal uppercase cta-section-description opacity-0">
            Move Dance Sport Academy's world-class shows transform any <br> occasion into an unforgettable experience, from exclusive private <br> gatherings to grand
            corporate galas.
          </p>
          <div class="mt-6 flex flex-col sm:flex-row justify-center gap-6">
            <button class="bg-[#FE2AF7] text-[16px] text-black uppercase font-bold py-4 px-20 shadow-lg cta-button opacity-0">
              Request a Call
            </button>
            <button class="bg-[#FE2AF7] text-[16px] text-black uppercase font-bold py-4 px-20 shadow-lg cta-button opacity-0">
              Request a Quote
            </button>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.coaching-section {
  position: relative;
  background-color: #000;
}

/* Gradient background animáció */
.gradient-bg {
  animation: gradientShift 8s ease-in-out infinite;
}

@keyframes gradientShift {
  0%, 100% {
    background: linear-gradient(180deg, rgba(0, 0, 0, 0) 55.37%, #000 100%);
  }
  50% {
    background: linear-gradient(180deg, rgba(0, 0, 0, 0) 45%, rgba(0, 188, 212, 0.1) 70%, #000 100%);
  }
}

/* Heading animációk */
.coaching-heading {
  transform: translateX(-60px) rotateY(-15deg);
  transition: all 1s cubic-bezier(0.25, 0.46, 0.45, 0.94);
  text-shadow: 0 0 30px rgba(0, 188, 212, 0.5);
}

.coaching-heading.animate-in {
  opacity: 1;
  transform: translateX(0) rotateY(0deg);
  text-shadow: 0 0 40px rgba(0, 188, 212, 0.7);
}

.coaching-description {
  transform: translateX(-40px);
  transition: all 0.8s cubic-bezier(0.25, 0.46, 0.45, 0.94) 0.3s;
}

.coaching-description.animate-in {
  opacity: 1;
  transform: translateX(0);
}

/* Coach kártya animációk */
.coach-card {
  transform: translateY(80px) scale(0.8);
  transition: all 0.9s cubic-bezier(0.34, 1.56, 0.64, 1) calc(0.5s + var(--coach-delay, 0s));
  cursor: pointer;
}

.coach-card.animate-in {
  opacity: 1;
  transform: translateY(0) scale(1);
}

/* Kép animációk */
.image-container {
  position: relative;
  overflow: hidden;
}

.coach-image {
  transform: scale(1.1);
  transition: transform 0.8s cubic-bezier(0.25, 0.46, 0.45, 0.94);
  filter: grayscale(30%) brightness(0.8);
}

.coach-card.animate-in .coach-image {
  transform: scale(1);
  filter: grayscale(0%) brightness(1);
}

/* Color overlay animáció */
.color-overlay {
  transform: scaleY(1.2);
  transition: all 0.8s cubic-bezier(0.25, 0.46, 0.45, 0.94) 0.2s;
}

.coach-card.animate-in .color-overlay {
  transform: scaleY(1);
}

/* Hover overlay effekt */
.image-hover-overlay {
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(45deg, transparent, rgba(254, 42, 247, 0.3), transparent);
  transition: left 0.8s ease;
  pointer-events: none;
}

.coach-card:hover .image-hover-overlay {
  left: 100%;
}

/* Text overlay animációk */
.text-overlay {
  transform: translateY(20px);
  transition: all 0.6s cubic-bezier(0.25, 0.46, 0.45, 0.94) 0.4s;
}

.coach-card.animate-in .text-overlay {
  transform: translateY(0);
}

/* Coach kártya hover effektek */
.coach-card:hover {
  transform: translateY(-15px) scale(1.05);
  transition: all 0.5s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}

.coach-card:hover .coach-image {
  transform: scale(1.08);
  filter: grayscale(0%) brightness(1.1) contrast(1.1);
}

.coach-card:hover .color-overlay {
  opacity: 0.7;
  background: linear-gradient(45deg, #00BCD4, #FE2AF7);
  mix-blend-mode: overlay;
}

.coach-card:hover .coach-name {
  color: #FE2AF7;
  text-shadow: 0 0 15px rgba(254, 42, 247, 0.6);
  transform: translateY(-2px);
}

.coach-card:hover .coach-role {
  color: #00BCD4;
  transform: translateY(-1px);
}

/* Szöveg animációk */
.coach-name {
  transition: all 0.3s ease;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.8);
}

.coach-role {
  transition: all 0.3s ease;
  text-shadow: 0 1px 3px rgba(0, 0, 0, 0.8);
}

/* CTA szekció animációk */
.cta-section-heading {
  transform: translateY(40px);
  transition: all 0.8s cubic-bezier(0.25, 0.46, 0.45, 0.94) 0.2s;
}

.cta-section-heading.animate-in {
  opacity: 1;
  transform: translateY(0);
}

.cta-section-description {
  transform: translateY(30px);
  transition: all 0.8s cubic-bezier(0.25, 0.46, 0.45, 0.94) 0.4s;
}

.cta-section-description.animate-in {
  opacity: 1;
  transform: translateY(0);
}

.cta-button {
  transform: translateY(30px) scale(0.9);
  transition: all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1) 0.6s;
  position: relative;
  overflow: hidden;
}

.cta-button.animate-in {
  opacity: 1;
  transform: translateY(0) scale(1);
}

/* CTA button hover effektek */
.cta-button:hover {
  transform: translateY(-5px) scale(1.05);
  box-shadow: 0 15px 35px rgba(254, 42, 247, 0.4);
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
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.3), transparent);
  transition: left 0.6s ease;
}

.cta-button:hover::before {
  left: 100%;
}

/* Grid responsive finomhangolások */
@media (max-width: 1024px) {
  .coach-card {
    transform: translateY(50px) scale(0.9);
  }
  
  .coach-card.animate-in {
    transform: translateY(0) scale(1);
  }
}

@media (max-width: 640px) {
  .coach-card {
    transform: translateY(30px);
  }
  
  .coach-card.animate-in {
    transform: translateY(0);
  }
  
  .coaching-heading {
    transform: translateY(40px);
  }
  
  .coaching-heading.animate-in {
    transform: translateY(0);
  }
}

/* Accessibility - prefers-reduced-motion */
@media (prefers-reduced-motion: reduce) {
  .gradient-bg {
    animation: none;
  }
  
  .coaching-heading,
  .coaching-description,
  .coach-card,
  .cta-section-heading,
  .cta-section-description,
  .cta-button {
    opacity: 1;
    transform: none;
    transition: none;
  }
  
  .coach-image {
    transform: scale(1);
    filter: grayscale(0%) brightness(1);
  }
  
  .color-overlay {
    transform: scaleY(1);
  }
  
  .text-overlay {
    transform: translateY(0);
  }
  
  .image-hover-overlay {
    display: none;
  }
  
  .cta-button::before {
    display: none;
  }
}

/* Extra vizuális effektek */
.coach-card::after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  border: 2px solid transparent;
  background: linear-gradient(45deg, transparent, rgba(0, 188, 212, 0.5), transparent) border-box;
  mask: linear-gradient(#fff 0 0) padding-box, linear-gradient(#fff 0 0);
  mask-composite: exclude;
  opacity: 0;
  transition: opacity 0.3s ease;
  pointer-events: none;
}

.coach-card:hover::after {
  opacity: 1;
}

/* Loading skeleton */
.coach-card:not(.animate-in) .coach-image {
  background: linear-gradient(90deg, #333, #555, #333);
  background-size: 200% 100%;
  animation: shimmerSkeleton 1.5s infinite;
}

@keyframes shimmerSkeleton {
  0% {
    background-position: -200% 0;
  }
  100% {
    background-position: 200% 0;
  }
}

.coach-card.animate-in .coach-image {
  animation: none;
}
</style>