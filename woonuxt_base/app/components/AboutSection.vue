<template>
  <section class="bg-gray-900 text-white about-section">
    <!-- Hero Heading -->
    <div class="container mx-auto px-4 py-12 text-center about-heading">
      <h1 class="text-3xl md:text-4xl font-extrabold uppercase">
        Step into the exclusive world of Move Dance Sport Academy Dubai,
      </h1>
      <p class="mt-4 text-lg">
        Where you can write the next chapter of acrobatic Rock 'n' Show alongside Europe's elite professionals!
      </p>
    </div>

    <!-- About Body: Full Width -->
    <div class="container mx-auto w-full px-0 grid grid-cols-1 md:grid-cols-2 about-body">
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
        <div class="relative p-10 flex flex-col justify-center">
          <h2 class="text-2xl md:text-3xl font-extrabold uppercase text-white">
            Introducing “MOVE”
          </h2>
          <p class="mt-4 leading-relaxed text-white">
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
  </section>
</template>

<style scoped>
.about-section {
  background: var(--Dark, #242424);
}
</style>
