<script setup>
import { ref, onMounted, onUnmounted, nextTick } from 'vue'

// Az aktuális események - csak az első 3 megjelenítése a főoldalon
const competitions = [
  { 
    title: 'Grand Opening - First Open Training', 
    location: 'Dubai, UAE', 
    date: 'Fri, 30 Aug',
    type: 'Grand Opening',
    image: '/img/upcomming/1.webp'
  },
  { 
    title: 'GEMS FirstPoint School - Sign-up day', 
    location: 'GEMS FirstPoint School', 
    date: 'Sun, 1 Sept',
    type: 'Sign-up',
    image: '/img/upcomming/2.png'
  },
  { 
    title: 'Raffles World Academy - First AACA training', 
    location: 'Raffles World Academy', 
    date: 'Sun, 8 Sept',
    type: 'Training',
    image: '/img/upcomming/3.png'
  }
];

const competitionsSection = ref(null)
const videoRef = ref(null)
const videoLoaded = ref(false)
const videoError = ref(false)
const isMobile = ref(false)

// Mobil eszköz detektálása
const detectMobile = () => {
  return /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent) ||
    window.innerWidth <= 768;
};

// Agresszív autoplay próbálkozások
const forceAutoplay = async (video) => {
  const playPromises = [];
  
  // 1. Alapvető play próbálkozás
  playPromises.push(
    video.play().catch(e => {
      console.log('Basic autoplay failed:', e.message);
      return null;
    })
  );

  // 2. Késleltetett próbálkozás
  playPromises.push(
    new Promise(resolve => {
      setTimeout(() => {
        video.play().then(resolve).catch(() => resolve(null));
      }, 100);
    })
  );

  // 3. Volume = 0 próbálkozás (ha muted nem működik)
  if (!video.muted) {
    video.volume = 0;
    playPromises.push(
      video.play().catch(() => null)
    );
  }

  // 4. User gesture szimuláció
  playPromises.push(
    new Promise(resolve => {
      const tryPlay = () => {
        video.play().then(resolve).catch(() => resolve(null));
        document.removeEventListener('touchstart', tryPlay);
        document.removeEventListener('touchend', tryPlay);
        document.removeEventListener('click', tryPlay);
      };
      
      document.addEventListener('touchstart', tryPlay, { once: true, passive: true });
      document.addEventListener('touchend', tryPlay, { once: true, passive: true });
      document.addEventListener('click', tryPlay, { once: true });
      
      // Timeout after 2 seconds
      setTimeout(() => resolve(null), 2000);
    })
  );

  return Promise.race(playPromises);
};

// Videó dinamikus betöltése
const loadVideo = async () => {
  if (!videoRef.value) return;

  try {
    const videoPaths = ['/video/ourword_reel.mp4'];
    let videoPath = null;

    for (const path of videoPaths) {
      try {
        const response = await fetch(path, { method: 'HEAD' });
        if (response.ok) {
          videoPath = path;
          break;
        }
      } catch (e) {
        continue;
      }
    }

    if (videoPath) {
      videoRef.value.src = videoPath;
      
      // Agresszív mobil beállítások
      if (isMobile.value) {
        videoRef.value.setAttribute('playsinline', '');
        videoRef.value.setAttribute('webkit-playsinline', '');
        videoRef.value.setAttribute('x-webkit-airplay', 'allow');
        videoRef.value.muted = true;
        videoRef.value.volume = 0;
        videoRef.value.defaultMuted = true;
        videoRef.value.controls = false;
        videoRef.value.disablePictureInPicture = true;
        videoRef.value.setAttribute('disableRemotePlayback', '');
      }

      videoRef.value.load();
      console.log(`Competition video loaded from: ${videoPath}`);
    } else {
      throw new Error('Video file not found in any expected location');
    }
  } catch (error) {
    console.warn('Competition video not available, using fallback image:', error);
    videoError.value = true;
  }
};

// Videó betöltődött
const onVideoLoaded = () => {
  if (videoRef.value && !videoError.value) {
    videoRef.value.currentTime = 13;
  }
};

// Videó készen áll
const onVideoCanPlay = async () => {
  if (videoRef.value && !videoError.value) {
    videoLoaded.value = true;
    
    try {
      // Agresszív autoplay kísérlet
      await forceAutoplay(videoRef.value);
      console.log('Competition video autoplay successful');
    } catch (e) {
      console.warn('All autoplay attempts failed:', e);
      // Itt sem adunk fel, próbálkozunk tovább
      continuousPlayAttempts();
    }
  }
};

// Folyamatos lejátszási kísérletek
const continuousPlayAttempts = () => {
  let attempts = 0;
  const maxAttempts = 20;
  
  const tryPlay = () => {
    if (attempts >= maxAttempts || !videoRef.value || videoError.value) return;
    
    attempts++;
    videoRef.value.play()
      .then(() => {
        console.log(`Competition video started after ${attempts} attempts`);
      })
      .catch(() => {
        setTimeout(tryPlay, 500); // Próbálkozás 500ms múlva
      });
  };
  
  tryPlay();
};

// Error handling
const onVideoError = (e) => {
  console.warn('Competition video loading failed, using fallback image', e);
  videoError.value = true;
  videoLoaded.value = false;
};

// Router navigáció a teljes lista oldalra
const navigateToAllEvents = () => {
  // Itt a router push-t használnád: $router.push('/competitions')
  console.log('Navigate to all competitions page')
}

// Refs a cleanup-hez
let observer = null;
let handleResize = null;

onMounted(async () => {
  await nextTick();
  
  isMobile.value = detectMobile();
  
  // Resize listener
  handleResize = () => {
    isMobile.value = detectMobile();
  };
  window.addEventListener('resize', handleResize);

  // Videó betöltése rövidebb késleltetéssel
  setTimeout(() => {
    loadVideo();
  }, 200);

  observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add('animate-in')
          
          // Videó lejátszás ha látható
          if (videoRef.value && videoLoaded.value && videoRef.value.paused) {
            videoRef.value.play().catch(() => {
              continuousPlayAttempts();
            });
          }
        } else {
          // Videó megállítása ha nem látható
          if (videoRef.value && videoLoaded.value) {
            videoRef.value.pause();
          }
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

  // Dokumentum interakciók figyelése
  const enableVideoOnInteraction = () => {
    if (videoRef.value && videoLoaded.value && videoRef.value.paused) {
      videoRef.value.play().catch(() => {});
    }
  };

  // Passzív event listener-ek
  document.addEventListener('touchstart', enableVideoOnInteraction, { passive: true });
  document.addEventListener('touchend', enableVideoOnInteraction, { passive: true });
  document.addEventListener('scroll', enableVideoOnInteraction, { passive: true });
  document.addEventListener('click', enableVideoOnInteraction, { passive: true });
})

// Cleanup - külön onUnmounted hook
onUnmounted(() => {
  if (observer) {
    observer.disconnect()
  }
  
  // Videó cleanup
  if (videoRef.value) {
    videoRef.value.pause();
    videoRef.value.removeAttribute('src');
    videoRef.value.load();
  }
  
  if (handleResize) {
    window.removeEventListener('resize', handleResize);
  }
  
  // Event listener cleanup
  const enableVideoOnInteraction = () => {};
  document.removeEventListener('touchstart', enableVideoOnInteraction);
  document.removeEventListener('touchend', enableVideoOnInteraction);
  document.removeEventListener('scroll', enableVideoOnInteraction);
  document.removeEventListener('click', enableVideoOnInteraction);
})
</script>

<template>
  <section class="relative text-white competitions-section" ref="competitionsSection" id="COMPETITIONS">
    <!-- Background Video -->
    <div class="absolute inset-0 bg-container">
      <!-- Háttér videó -->
      <video 
        ref="videoRef"
        class="w-full h-full object-cover bg-video competition-video" 
        :class="{ 'mobile-video': isMobile }"
        muted 
        playsinline 
        webkit-playsinline
        autoplay 
        loop
        :preload="isMobile ? 'auto' : 'auto'"
        disablePictureInPicture
        x-webkit-airplay="allow"
        :style="{ objectFit: 'cover', objectPosition: 'center' }"
        @loadeddata="onVideoLoaded"
        @error="onVideoError"
        @canplay="onVideoCanPlay"
        @loadedmetadata="onVideoCanPlay"
      >
        <!-- Fallback kép ha a videó nem töltődik be -->
      </video>
      
      <!-- Fallback kép -->
      <NuxtImg
        v-show="!videoLoaded || videoError"
        src="/img/com.webp"
        format="webp"
        class="absolute inset-0 object-cover w-full h-full pointer-events-none competition-bg"
        alt="Competitions background" />
      
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
          <div class="w-20 h-20 bg-gray-200 rounded mr-6 flex-shrink-0 competition-icon overflow-hidden">
            <!-- Event képek -->
            <NuxtImg
              :src="comp.image"
              :alt="`${comp.title} image`"
              class="w-full h-full object-cover rounded transition-transform duration-300 hover:scale-110"
              loading="lazy"
            />
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

/* Háttér videó */
.bg-container {
  overflow: hidden;
  min-height: 100vh;
}

.bg-video {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
  min-width: 100%;
  min-height: 100%;
}

/* Videó optimalizálás - hero-ból átvéve */
.competition-video {
  transform: scale(1.02);
  will-change: transform;
  transform: translateZ(0);
  backface-visibility: hidden;
  opacity: 0;
  animation: fadeInVideo 1s ease-out 0.2s forwards;
}

/* Mobil videó specifikus stílusok */
.mobile-video {
  transform: scale(1.1);
  object-position: center center;
}

@keyframes fadeInVideo {
  to {
    opacity: 1;
  }
}

/* Fallback animáció */
.competition-bg {
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

/* Safari és iOS specifikus videó javítások */
@supports (-webkit-appearance: none) {
  .bg-video {
    -webkit-transform: translateZ(0);
    transform: translateZ(0);
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

/* Icon animációk - most képekre módosítva */
.competition-icon {
  transform: scale(0) rotate(-180deg);
  transition: all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1) calc(0.8s + var(--item-delay, 0s));
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}

.competition-item.animate-in .competition-icon {
  transform: scale(1) rotate(0deg);
}

/* Kép hover effekt */
.competition-icon:hover {
  transform: scale(1.05) rotate(2deg);
  box-shadow: 0 8px 25px rgba(0, 188, 212, 0.3);
  transition: all 0.3s cubic-bezier(0.25, 0.46, 0.45, 0.94);
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
  
  .competition-video {
    transform: scale(1.15);
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

/* Alacsony sávszélesség - csak nagyon régi eszközökön */
@media (max-width: 360px) and (max-resolution: 1dppx) {
  .competition-video {
    display: none !important;
  }

  .competition-bg {
    display: block !important;
  }
}

/* Accessibility - prefers-reduced-motion */
@media (prefers-reduced-motion: reduce) {
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
  
  .competition-date::after {
    display: none;
  }
  
  .show-all-button::before {
    display: none;
  }
  
  .competition-video {
    animation: none !important;
    opacity: 1;
    transform: scale(1.02);
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

/* Performance hints */
.competition-video {
  transform: translateZ(0);
  backface-visibility: hidden;
}
</style>