<script setup>
const coaches = [
  { name: 'Tamás Kuptsulik', role: 'Academy Director / Coach', photo: '/img/tamas.webp' },
  { name: 'Veronika Lengyel', role: 'Academy Co-owner / Coach', photo: '/img/veronika.webp' },
  { name: 'Gábor Mészáros', role: 'Academy Co-owner / Coach', photo: '/img/gabor.webp' },
  { name: 'Bonita Lengyel Mészárosné', role: 'Coach', photo: '/img/boni.webp' },
  { name: 'Patrik Toma', role: 'Coach', photo: '/img/patrik.webp' },
  { name: 'Máté Hollóssy', role: 'Coach', photo: '/img/mate.webp' },
];
</script>

<template>
  <section class="relative text-white pt-16 coaching-section">
    <!-- Gradient Background -->
    <div class="absolute inset-0 mix-blend-multiply" style="background: linear-gradient(180deg, rgba(0, 0, 0, 0) 55.37%, #000 100%)"></div>

    <div class="relative container mx-auto pt-4 px-4">
      <!-- Heading (left aligned) -->
      <div class="mb-12 text-left max-w-3xl">
        <h2 class="font-unbounded uppercase text-cyan-400 font-medium text-[20px] lg:text-[32px]">Our Elite <br> Coaching Team</h2>
        <p class="mt-4 text-lg leading-relaxed text-[16px]">
          Our coaches are living legends of the sport! Their competitive dancers have claimed 26 World Championship titles in the past 13 years under the WRRC
          (World Rock'n'Roll Confederation) – an unprecedented achievement in the history of acrobatic rock 'n' roll!
        </p>
      </div>

      <!-- Team Grid with larger gap and taller images -->
      <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-6 gap-1">
        <div v-for="(coach, i) in coaches" :key="i" class="relative overflow-hidden">
          <!-- Image with color overlay and fixed height -->
          <div class="relative">
            <NuxtImg :src="coach.photo" :alt="coach.name" class="w-full sm:h-full lg:h-[450px] object-cover" />
            <div class="absolute inset-0 bg-cyan-400 opacity-100 mix-blend-multiply"></div>
          </div>
          <!-- Text block -->
          <div class="absolute bottom-0 left-0 w-full p-4 bg-gradient-to-t from-black to-transparent">
            <h3 class="text-[20px] font-unbounded font-medium uppercase">
              {{ coach.name }}
            </h3>
            <p class="text-[14px] uppercase mt-1">
              {{ coach.role }}
            </p>
          </div>
        </div>
      </div>
    </div>
    <!-- Event Performance CTA Container -->
    <div class="relative bg-[#242424]">
      <div class="container mx-auto px-4 py-12">
        <div class="text-center">
          <h3 class="text-[20px] lg:text-[20px] font-unbounded uppercase font-bold">Elevate your event with a breathtaking <br> acrobatic Rock 'n' Show performance!</h3>
          <p class="mt-2 text-[16px] lg:text-[16px] font-unbounded font-normal uppercase">
            Move Dance Sport Academy's world-class shows transform any <br> occasion into an unforgettable experience, from exclusive private <br> gatherings to grand
            corporate galas.
          </p>
          <div class="mt-6 flex flex-col sm:flex-row justify-center gap-6">
            <button class="bg-[#FE2AF7] text-[16px] text-black uppercase font-bold py-4 px-20 shadow-lg">Request a Call</button>
            <button class="bg-[#FE2AF7] text-[16px] text-black uppercase font-bold py-4 px-20 shadow-lg">Request a Quote</button>
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
</style>
