<script setup>
const coaches = [
  { name: 'Tamás Kuptsulik', role: 'Academy Director / Coach', photo: '/img/about.svg' },
  { name: 'Veronika Lengyel', role: 'Academy Co-owner / Coach', photo: '/img/about.svg' },
  { name: 'Gábor Mészáros', role: 'Academy Co-owner / Coach', photo: '/img/about.svg' },
  { name: 'Bonita Lengyel Mészárosné', role: 'Coach', photo: '/img/about.svg' },
  { name: 'Patrik Toma', role: 'Coach', photo: '/img/about.svg' },
  { name: 'Máté Hollóssy', role: 'Coach', photo: '/img/about.svg' },
];
</script>

<template>
  <section class="relative text-white pt-16 coaching-section">
    <!-- Gradient Background -->
    <div class="absolute inset-0 mix-blend-multiply" style="background: linear-gradient(180deg, rgba(0, 0, 0, 0) 55.37%, #000 100%)"></div>

    <div class="relative container mx-auto px-4">
      <!-- Heading (left aligned) -->
      <div class="mb-12 text-left max-w-3xl">
        <h2 class="text-3xl md:text-4xl font-extrabold uppercase text-cyan-400">Our Elite Coaching Team</h2>
        <p class="mt-4 text-lg leading-relaxed">
          Our coaches are living legends of the sport! Their competitive dancers have claimed 26 World Championship titles in the past 13 years under the WRRC
          (World Rock'n'Roll Confederation) – an unprecedented achievement in the history of acrobatic rock 'n' roll!
        </p>
      </div>

      <!-- Team Grid with larger gap and taller images -->
      <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-6 gap-10">
        <div v-for="(coach, i) in coaches" :key="i" class="relative overflow-hidden rounded-lg">
          <!-- Image with color overlay and fixed height -->
          <div class="relative h-80">
            <NuxtImg :src="coach.photo" :alt="coach.name" class="w-full h-full object-cover" />
            <div class="absolute inset-0 bg-cyan-400 opacity-60 mix-blend-multiply"></div>
          </div>
          <!-- Text block -->
          <div class="absolute bottom-0 left-0 w-full p-4 bg-gradient-to-t from-black to-transparent">
            <h3 class="text-xl font-extrabold uppercase">
              {{ coach.name }}
            </h3>
            <p class="text-sm uppercase mt-1">
              {{ coach.role }}
            </p>
          </div>
        </div>
      </div>
    </div>
    <!-- Event Performance CTA Container -->
    <div class="relative mt-16 bg-[#242424]">
      <div class="container mx-auto px-4 py-12">
        <div class="text-center">
          <h3 class="text-3xl md:text-4xl font-extrabold uppercase text-white">Elevate your event with a breathtaking acrobatic Rock 'n' Show performance!</h3>
          <p class="mt-4 max-w-3xl mx-auto text-lg leading-relaxed text-white">
            Move Dance Sport Academy's world-class shows transform any occasion into an unforgettable experience, from exclusive private gatherings to grand
            corporate galas.
          </p>
          <div class="mt-6 flex flex-col sm:flex-row justify-center gap-6">
            <button class="bg-[#FE2AF7] text-black uppercase font-bold py-4 px-8 rounded-lg shadow-lg">Request a Call</button>
            <button class="bg-[#FE2AF7] text-black uppercase font-bold py-4 px-8 rounded-lg shadow-lg">Request a Quote</button>
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
