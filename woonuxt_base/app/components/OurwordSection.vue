<script setup>
const champions = [
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2012' },
  { category: 'Soldiers Junior Couple Formation', names: 'Ádám Király & Dorottya Novák', year: '2013' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2013' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2014' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2014' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2015' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2016' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2018' },
];
</script>


<template>
  <section class="relative text-white pt-16">
    <!-- Background Image & Overlay -->
    <div class="absolute inset-0">
      <NuxtImg src="/img/about.svg" alt="Champions background" class="w-full h-full object-cover" />
      <div class="absolute inset-0 bg-black opacity-75"></div>
    </div>

    <!-- Main Content -->
    <div class="relative container mx-auto px-4">
      <!-- Heading -->
      <div class="text-center mb-12">
        <h2 class="text-3xl md:text-4xl font-extrabold uppercase text-cyan-400">Our World Champions</h2>
        <p class="mt-4 max-w-2xl mx-auto text-lg leading-relaxed">
          Our coaches are living legends of the sport! Their competitive dancers have claimed 26 World Championship titles in the past 13 years under the WRRC
          (World Rock'n'Roll Confederation) – an unprecedented achievement in the history of acrobatic rock 'n' roll!
        </p>
      </div>

      <!-- Cards Grid -->
      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
        <div v-for="(item, index) in champions" :key="index" class="bg-white text-[#242424] p-6 rounded-lg shadow-lg flex flex-col justify-between">
          <div>
            <h3 class="text-lg font-bold uppercase">
              {{ item.category }}
            </h3>
            <p class="mt-2">
              {{ item.names }}
            </p>
          </div>
          <div class="mt-4">
            <span class="text-5xl md:text-6xl font-extrabold">
              {{ item.year }}
            </span>
          </div>
        </div>
      </div>
    </div>

    <!-- Training Programs CTA Container -->
    <div class="relative mt-16" style="background: var(--Dark, #242424)">
      <div class="container mx-auto px-4 py-12">
        <div class="text-center">
          <h3 class="text-3xl md:text-4xl font-extrabold uppercase text-white">Explore our<br />Training Programs</h3>
          <div class="inline-flex items-center mt-6">
            <button class="bg-[#FE2AF7] text-black uppercase font-bold py-4 px-8 rounded-lg shadow-lg">Coming Soon</button>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
