<template>
  <section class="bg-gray-900 text-white about-section" ref="aboutSection">
    <!-- Hero Heading -->
    <div class="container mx-auto px-4 py-12 text-center about-heading">
      <h4 class="text-[20px] lg:text-[20px] font-unbounded uppercase font-bold heading-main opacity-0">
        Step into the exclusive world of <br> Move Dance Sport Academy Dubai,
      </h4>
      <p class="mt-2 text-[16px] lg:text-[16px] font-unbounded uppercase font-normal heading-sub opacity-0">
        Where you can write the next chapter of acrobatic <br> Rock 'n' Show alongside Europe's elite professionals!
      </p>
    </div>

    <!-- About Body: Full Width -->
    <div class="w-screen relative left-1/2 right-1/2 -ml-[50vw] -mr-[50vw] about-body">
      <div class="grid grid-cols-1 md:grid-cols-2">
        <!-- Illustration / Image -->
        <div class="w-full image-container opacity-0">
          <NuxtImg
            src="/img/about.svg"
            alt="Move Dance Sport Academy"
            class="w-full h-full object-cover shadow-md about-image"
          />
          <div class="image-overlay"></div>
        </div>

        <!-- Text Content with Overlay Background -->
        <div class="w-full relative">
          <!-- Colored overlay with multiply -->
          <div class="absolute inset-0 bg-[#A700AB] mix-blend-multiply"></div>
          <!-- Text wrapper ensures normal blend mode for text -->
          <div class="relative p-6 lg:p-10 xl:p-20 flex flex-col justify-center max-w-4xl mx-auto">
            <h5 class="text-[21px] lg:text-[32px] uppercase font-unbounded font-medium" style="color: #FA3DFF;">
              Introducing "MOVE"
            </h5>
            <p class="mt-4 leading-relaxed text-white text-[16px] lg:text-[16px] font-medium">
              At Move Dance Sport Academy, our philosophy is clear: Acrobatic Rock 'n' Show is not just dance - it's a complex, dynamic sport that combines artistry with athletic excellence. Our mission is to provide professional sports training for all age groups, blending it with the joy of dance. We conduct our training sessions at strategic locations across Dubai's finest schools to ensure maximum accessibility for everyone.
            </p>
            <p class="mt-4 leading-relaxed text-white">
              Our Academy, founded by Tamas Kuptsulik and co‑founded by Veronika Lengyel and Gabor Meszaros – all multiple World Champion coaches and directors of Europe's leading clubs – guarantees the highest professional standards.
            </p>
            <p class="mt-4 leading-relaxed text-white">
              Move Dance Sport Academy's athletes belong to the international elite of the sport, with our couples and formations regularly claiming top positions on the podium.
            </p>
            <p class="mt-4 leading-relaxed text-white">
              This expertise and experience forms the foundation for the United Arab Emirates to soon become the new, defining center of Acrobatic Rock 'n' Show/Roll competitive sport and education.
            </p>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const aboutSection = ref(null)

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

  const elementsToAnimate = aboutSection.value?.querySelectorAll(
    '.heading-main, .heading-sub, .image-container'
  )

  elementsToAnimate?.forEach((el) => {
    observer.observe(el)
  })

  onUnmounted(() => {
    observer.disconnect()
  })
})
</script>

<style scoped>
.about-section {
  background: var(--Dark, #242424) !important;
  background-color: #242424;
}

/* Fő heading animációk */
.heading-main {
  transform: translateY(30px);
  transition: all 0.8s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}

.heading-main.animate-in {
  opacity: 1;
  transform: translateY(0);
}

.heading-sub {
  transform: translateY(40px);
  transition: all 0.9s cubic-bezier(0.25, 0.46, 0.45, 0.94) 0.2s;
}

.heading-sub.animate-in {
  opacity: 1;
  transform: translateY(0);
}

/* Kép container animáció */
.image-container {
  transform: translateX(-50px);
  transition: all 1s cubic-bezier(0.25, 0.46, 0.45, 0.94) 0.3s;
  position: relative;
  overflow: hidden;
}

.image-container.animate-in {
  opacity: 1;
  transform: translateX(0);
}

/* Kép animációk */
.about-image {
  transform: scale(1.1);
  transition: transform 1.2s cubic-bezier(0.25, 0.46, 0.45, 0.94) 0.3s;
  filter: grayscale(20%) brightness(0.9);
}

.image-container.animate-in .about-image {
  transform: scale(1);
  filter: grayscale(0%) brightness(1);
}

/* Kép overlay effekt */
.image-overlay {
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(250, 61, 255, 0.3), transparent);
  transition: left 1s cubic-bezier(0.25, 0.46, 0.45, 0.94) 0.8s;
  pointer-events: none;
}

.image-container.animate-in .image-overlay {
  left: 100%;
}

/* Szöveg container animáció */
.text-container {
  transform: translateX(50px);
  transition: all 1s cubic-bezier(0.25, 0.46, 0.45, 0.94) 0.5s;
}

.text-container.animate-in {
  opacity: 1;
  transform: translateX(0);
}

/* Overlay background animáció */
.overlay-bg {
  transform: scaleX(0);
  transform-origin: left;
  transition: transform 1.2s cubic-bezier(0.25, 0.46, 0.45, 0.94) 0.5s;
}

.text-container.animate-in .overlay-bg {
  transform: scaleX(1);
}

/* További finomítások */
p {
  line-height: 1.7;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.3);
}
</style>