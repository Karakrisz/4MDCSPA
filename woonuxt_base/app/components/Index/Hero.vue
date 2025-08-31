<script setup>
import { ref, onMounted, onUnmounted, nextTick } from 'vue';

const heroVideo = ref(null);
const videoLoaded = ref(false);
const videoError = ref(false);

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
        // Folytatjuk a következő útvonallal
        continue;
      }
    }

    if (videoPath) {
      // Ha megtaláltuk, betöltjük
      const source = document.createElement('source');
      source.src = videoPath;
      source.type = 'video/mp4';
      heroVideo.value.appendChild(source);

      // Betöltés indítása
      heroVideo.value.load();
      console.log(`Video loaded from: ${videoPath}`);
    } else {
      throw new Error('Video file not found in any expected location');
    }
  } catch (error) {
    console.warn('Video not available, using fallback image:', error);
    console.warn('Expected video locations:', ['public/video/slide_reel.mp4', 'public/video/silde_reel.mp4', 'public/assets/video/slide_reel.mp4']);
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
    heroVideo.value.play().catch((e) => {
      console.warn('Video autoplay failed:', e);
      videoError.value = true;
    });
  }
};

// Error handling - fallback képre vált
const onVideoError = () => {
  console.warn('Video loading failed, using fallback image');
  videoError.value = true;
  videoLoaded.value = false;
};

// Performance optimalizálás
onMounted(async () => {
  await nextTick();

  // Videó betöltése késleltetéssel (hogy ne blokkolja az oldalt)
  setTimeout(() => {
    loadVideo();
  }, 500);

  // Intersection Observer a videó optimális lejátszásához
  if ('IntersectionObserver' in window && heroVideo.value) {
    const observer = new IntersectionObserver(
      (entries) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting && heroVideo.value && videoLoaded.value) {
            heroVideo.value.play().catch(() => {
              videoError.value = true;
            });
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
      muted
      loop
      playsinline
      preload="none"
      @loadeddata="onVideoLoaded"
      @error="onVideoError"
      @canplay="onVideoCanPlay">
      <!-- Dinamikusan betöltjük a videót -->
    </video>

    <!-- Fallback kép ha a videó nem tölt be vagy még betöltődik -->
    <NuxtImg
      v-show="!videoLoaded"
      src="/img/hero.svg"
      format="webp"
      class="absolute inset-0 object-cover w-full h-full pointer-events-none hero-bg"
      alt="Hero background" />

    <!-- Backup/kommentezett képes háttér -->
    <!-- 
        <NuxtImg 
            src="/img/hero.svg" 
            format="webp" 
            class="absolute inset-0 object-cover w-full h-full pointer-events-none hero-bg"
        />
        -->

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
  transform: scale(1.02); /* Kis zoom a fekete sávok elkerüléséhez */
  will-change: transform; /* GPU gyorsítás */
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

/* Fallback animáció (ha nincs videó) */
.hero-bg {
  animation: backgroundFloat 20s ease-in-out infinite;
}

@keyframes backgroundFloat {
  0%,
  100% {
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
    opacity: 0.4; /* Kicsit átlátszóbb a videó miatt */
  }
}

/* Welcome szöveg animáció */
.welcome-text {
  animation: slideInFromTop 1s ease-out 1s forwards;
}

/* Fő cím animáció */
.main-title {
  animation: slideInScale 1.2s ease-out 1.4s forwards;
  transform: translateY(30px) scale(0.9);
}

/* Alcím animáció */
.subtitle {
  animation: slideInFromBottom 1s ease-out 1.8s forwards;
  transform: translateY(50px);
}

/* Logó animáció */
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

/* Hover effekt a szövegekre */
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
  0%,
  100% {
    text-shadow: 0 0 5px rgba(255, 255, 255, 0.5);
  }
  50% {
    text-shadow: 0 0 20px rgba(255, 255, 255, 0.8);
  }
}

@keyframes titlePulse {
  0%,
  100% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.05);
  }
}

@keyframes subtitleShake {
  0%,
  100% {
    transform: translateX(0);
  }
  25% {
    transform: translateX(-3px);
  }
  75% {
    transform: translateX(3px);
  }
}

/* PWA optimalizálások */
@media (max-width: 768px) {
  .hero-video {
    transform: scale(1.1); /* Mobil eszközökön nagyobb zoom */
  }

  /* Gyorsabb animációk mobilon */
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

/* Alacsony sávszélességnél videó kikapcsolása */
@media (max-width: 480px) and (max-resolution: 1dppx) {
  .hero-video {
    display: none;
  }

  .hero-bg {
    display: block !important;
  }
}

/* Prefers-reduced-motion támogatás */
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

/* Dark mode támogatás */
@media (prefers-color-scheme: dark) {
  .overlay {
    background: #0a0a0a;
  }
}

/* További finomhangolások */
.main-title {
  letter-spacing: 2px;
  text-shadow: 2px 2px 8px rgba(0, 0, 0, 0.8); /* Erősebb árnyék videó miatt */
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
  transform: translateZ(0); /* Hardware acceleration */
  backface-visibility: hidden;
}
</style>
