<script setup>
import { ref, onMounted, onUnmounted, nextTick } from 'vue';

const heroVideo = ref(null);
const videoLoaded = ref(false);
const videoError = ref(false);
const isMobile = ref(false);
const userInteracted = ref(false);

// Mobil eszköz detektálása
const detectMobile = () => {
  return /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent) ||
    window.innerWidth <= 768;
};

// Videó dinamikus betöltése
const loadVideo = async () => {
  if (!heroVideo.value) return;

  try {
    // Különböző elérési utak kipróbálása
    const videoPaths = ['/video/slide_reel.mp4'];

    let videoPath = null;

    // Próbáljuk meg megtalálni a videót
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
      // Ha megtaláltuk, betöltjük
      heroVideo.value.src = videoPath;
      
      // Mobil eszközökön más stratégia
      if (isMobile.value) {
        heroVideo.value.controls = false;
        heroVideo.value.playsInline = true;
        heroVideo.value.muted = true;
        heroVideo.value.autoplay = false; // Mobil eszközökön kikapcsoljuk az autoplay-t
      }

      // Betöltés indítása
      heroVideo.value.load();
      console.log(`Video loaded from: ${videoPath}`);
    } else {
      throw new Error('Video file not found in any expected location');
    }
  } catch (error) {
    console.warn('Video not available, using fallback image:', error);
    videoError.value = true;
  }
};

// Videó 8 másodpercnél kezdése
const onVideoLoaded = () => {
  if (heroVideo.value && !videoError.value) {
    heroVideo.value.currentTime = 8;
  }
};

// Videó készen áll a lejátszásra
const onVideoCanPlay = () => {
  if (heroVideo.value && !videoError.value) {
    videoLoaded.value = true;
    
    // Desktop esetén próbáljuk az autoplay-t
    if (!isMobile.value) {
      heroVideo.value.play().catch((e) => {
        console.warn('Video autoplay failed:', e);
        videoError.value = true;
      });
    } else {
      // Mobil esetén várjuk a user interakciót
      setupMobileVideoInteraction();
    }
  }
};

// Mobil videó interakció beállítása
const setupMobileVideoInteraction = () => {
  const playVideoOnInteraction = () => {
    if (heroVideo.value && !userInteracted.value && videoLoaded.value) {
      userInteracted.value = true;
      heroVideo.value.play().catch((e) => {
        console.warn('Mobile video play failed:', e);
        videoError.value = true;
      });
      
      // Event listener eltávolítása
      document.removeEventListener('touchstart', playVideoOnInteraction);
      document.removeEventListener('click', playVideoOnInteraction);
      document.removeEventListener('scroll', playVideoOnInteraction);
    }
  };

  // Különböző interakciókra várunk
  document.addEventListener('touchstart', playVideoOnInteraction, { once: true, passive: true });
  document.addEventListener('click', playVideoOnInteraction, { once: true });
  document.addEventListener('scroll', playVideoOnInteraction, { once: true, passive: true });
};

// Error handling
const onVideoError = (e) => {
  console.warn('Video loading failed, using fallback image', e);
  videoError.value = true;
  videoLoaded.value = false;
};

// Performance optimalizálás
onMounted(async () => {
  await nextTick();
  
  // Mobil detektálás
  isMobile.value = detectMobile();
  
  // Resize listener
  const handleResize = () => {
    isMobile.value = detectMobile();
  };
  window.addEventListener('resize', handleResize);

  // Videó betöltése késleltetéssel
  setTimeout(() => {
    loadVideo();
  }, isMobile.value ? 1000 : 500); // Mobil eszközökön még hosszabb késleltetés

  // Intersection Observer a videó optimális lejátszásához
  if ('IntersectionObserver' in window && heroVideo.value) {
    const observer = new IntersectionObserver(
      (entries) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting && heroVideo.value && videoLoaded.value) {
            // Desktop esetén vagy ha már volt interakció
            if (!isMobile.value || userInteracted.value) {
              heroVideo.value.play().catch(() => {
                videoError.value = true;
              });
            }
          } else if (heroVideo.value && videoLoaded.value) {
            heroVideo.value.pause();
          }
        });
      },
      { threshold: 0.25 },
    );

    observer.observe(heroVideo.value);
  }
});

// Cleanup
onUnmounted(() => {
  if (heroVideo.value) {
    heroVideo.value.pause();
    heroVideo.value.removeAttribute('src');
    heroVideo.value.load();
  }
});
</script>

<template>
  <section class="relative z-10 w-full h-[800px] overflow-hidden">
    <!-- Videós háttér -->
    <video
      ref="heroVideo"
      class="absolute inset-0 object-cover w-full h-full pointer-events-none hero-video"
      :class="{ 'mobile-video': isMobile }"
      muted
      loop
      playsinline
      :preload="isMobile ? 'metadata' : 'auto'"
      webkit-playsinline
      @loadeddata="onVideoLoaded"
      @error="onVideoError"
      @canplay="onVideoCanPlay">
    </video>

    <!-- Mobil videó play gomb (opcionális) -->
    <div 
      v-if="isMobile && videoLoaded && !userInteracted && !videoError"
      class="absolute inset-0 flex items-center justify-center z-20 bg-black bg-opacity-30"
      @click="setupMobileVideoInteraction">
      <button class="video-play-btn">
        <svg width="80" height="80" viewBox="0 0 24 24" fill="white">
          <path d="M8 5v14l11-7z"/>
        </svg>
      </button>
    </div>

    <!-- Fallback kép -->
    <NuxtImg
      v-show="!videoLoaded || videoError"
      src="/img/hero.svg"
      format="webp"
      class="absolute inset-0 object-cover w-full h-full pointer-events-none hero-bg"
      alt="Hero background" />

    <!-- Overlay animációval -->
    <div class="absolute inset-0 overlay" style="background: #141414"></div>

    <!-- Fő tartalom -->
    <div class="container relative z-10 h-full flex items-center justify-center flex-col">
      <div class="text-center text-white">
        <!-- Welcome szöveg -->
        <h3 class="text-lg mb-4 font-unbounded welcome-text opacity-0">Welcome to the world of</h3>

        <!-- Fő címsor -->
        <h1 class="text-5xl font-bold mb-2 font-unbounded text-[32px] lg:text-[64px] main-title opacity-0">MOVE DANCE</h1>

        <!-- Alcímsor -->
        <h2 class="text-3xl mb-8 font-unbounded text-[20px] lg:text-[32px] subtitle opacity-0">SPORT ACADEMY</h2>
      </div>
    </div>

    <!-- Animált logó -->
    <img src="/img/logo.png" alt="Logo" class="absolute mb-10 bottom-0 left-1/2 transform -translate-x-1/2 w-[100px] logo opacity-0" />
  </section>
</template>

<style scoped>
/* Videó optimalizálás */
.hero-video {
  transform: scale(1.02);
  will-change: transform;
}

/* Mobil videó specifikus stílusok */
.mobile-video {
  transform: scale(1.1);
  object-position: center center;
}

/* Video play button stílus */
.video-play-btn {
  background: rgba(0, 0, 0, 0.5);
  border: 2px solid white;
  border-radius: 50%;
  width: 100px;
  height: 100px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.3s ease;
}

.video-play-btn:hover {
  background: rgba(0, 0, 0, 0.7);
  transform: scale(1.1);
}

/* Smooth videó betöltés */
.hero-video {
  opacity: 0;
  animation: fadeInVideo 1s ease-out 0.2s forwards;
}

@keyframes fadeInVideo {
  to {
    opacity: 1;
  }
}

/* Fallback animáció */
.hero-bg {
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

/* Overlay fade-in */
.overlay {
  opacity: 0;
  animation: fadeInOverlay 1s ease-out 0.5s forwards;
}

@keyframes fadeInOverlay {
  to {
    opacity: 0.4;
  }
}

/* Szöveg animációk */
.welcome-text {
  animation: slideInFromTop 1s ease-out 1s forwards;
}

.main-title {
  animation: slideInScale 1.2s ease-out 1.4s forwards;
  transform: translateY(30px) scale(0.9);
}

.subtitle {
  animation: slideInFromBottom 1s ease-out 1.8s forwards;
  transform: translateY(50px);
}

.logo {
  animation: bounceInLogo 1.5s cubic-bezier(0.68, -0.55, 0.265, 1.55) 2.2s forwards;
  transform: translateX(-50%) translateY(50px) scale(0.5);
}

/* Animáció definíciók */
@keyframes slideInFromTop {
  from {
    opacity: 0;
    transform: translateY(-30px);
  }
  to {
    opacity: 1;
    transform: translateY(0px);
  }
}

@keyframes slideInScale {
  from {
    opacity: 0;
    transform: translateY(30px) scale(0.9);
  }
  to {
    opacity: 1;
    transform: translateY(0px) scale(1);
  }
}

@keyframes slideInFromBottom {
  from {
    opacity: 0;
    transform: translateY(50px);
  }
  to {
    opacity: 1;
    transform: translateY(0px);
  }
}

@keyframes bounceInLogo {
  from {
    opacity: 0;
    transform: translateX(-50%) translateY(50px) scale(0.5);
  }
  to {
    opacity: 1;
    transform: translateX(-50%) translateY(0px) scale(1);
  }
}

/* Hover effektek */
.welcome-text:hover {
  animation: textGlow 0.3s ease-in-out;
}

.main-title:hover {
  animation: titlePulse 0.5s ease-in-out;
}

.subtitle:hover {
  animation: subtitleShake 0.6s ease-in-out;
}

@keyframes textGlow {
  0%, 100% {
    text-shadow: 0 0 5px rgba(255, 255, 255, 0.5);
  }
  50% {
    text-shadow: 0 0 20px rgba(255, 255, 255, 0.8);
  }
}

@keyframes titlePulse {
  0%, 100% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.05);
  }
}

@keyframes subtitleShake {
  0%, 100% {
    transform: translateX(0);
  }
  25% {
    transform: translateX(-3px);
  }
  75% {
    transform: translateX(3px);
  }
}

/* Mobil optimalizálások */
@media (max-width: 768px) {
  .hero-video {
    transform: scale(1.15);
  }

  .main-title {
    animation-delay: 1.2s;
  }

  .subtitle {
    animation-delay: 1.6s;
  }

  .logo {
    animation-delay: 2s;
  }
}

/* Nagyon alacsony sávszélesség */
@media (max-width: 480px) and (max-resolution: 1dppx) {
  .hero-video {
    display: none;
  }

  .hero-bg {
    display: block !important;
  }
}

/* Reduced motion támogatás */
@media (prefers-reduced-motion: reduce) {
  .hero-video,
  .hero-bg,
  .welcome-text,
  .main-title,
  .subtitle,
  .logo {
    animation: none !important;
    opacity: 1;
    transform: none;
  }

  .overlay {
    opacity: 0.4;
  }

  .logo {
    transform: translateX(-50%);
  }

  .hero-video {
    transform: scale(1.02);
  }
}

/* Dark mode */
@media (prefers-color-scheme: dark) {
  .overlay {
    background: #0a0a0a;
  }
}

/* További finomhangolások */
.main-title {
  letter-spacing: 2px;
  text-shadow: 2px 2px 8px rgba(0, 0, 0, 0.8);
}

.subtitle {
  letter-spacing: 1px;
  text-shadow: 1px 1px 6px rgba(0, 0, 0, 0.8);
}

.welcome-text {
  text-shadow: 1px 1px 4px rgba(0, 0, 0, 0.7);
}

.logo {
  filter: drop-shadow(0 4px 12px rgba(0, 0, 0, 0.6));
  transition: filter 0.3s ease;
}

.logo:hover {
  filter: drop-shadow(0 6px 16px rgba(255, 255, 255, 0.3));
}

/* Performance hints */
.hero-video {
  transform: translateZ(0);
  backface-visibility: hidden;
}
</style>