<template>
  <section class="bg-gray-900 text-white about-section">
    <!-- Hero Heading -->
    <div class="container mx-auto px-4 py-12 text-center about-heading">
      <h4 class="text-[20px] lg:text-[20px] font-unbounded uppercase font-bold">
        Step into the exclusive world of <br> Move Dance Sport Academy Dubai,
      </h4>
      <p class="mt-2 text-[16px] lg:text-[16px] font-unbounded uppercase font-normal">
        Where you can write the next chapter of acrobatic <br> Rock 'n' Show alongside Europe's elite professionals!
      </p>
    </div>

    <!-- About Body: Full Width -->
    <div class="w-screen relative left-1/2 right-1/2 -ml-[50vw] -mr-[50vw] about-body">
      <div class="grid grid-cols-1 md:grid-cols-2">
        <!-- Illustration / Image -->
        <div class="w-full">
          <NuxtImg
            src="/img/about.svg"
            alt="Move Dance Sport Academy"
            class="w-full h-full object-cover shadow-md"
          />
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

<style scoped>
.about-section {
  background: var(--Dark, #242424);
}
</style>