<script setup>
const champions = [
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2012' },
  { category: 'Soldiers Junior Couple Formation', names: '', year: '2013' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2013' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2014' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2014' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2015' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2016' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2018' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2019' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2019' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2019' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2019' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2021' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2021' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2021' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2021' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2022' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2022' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2022' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2022' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2022' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2022' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2023' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2023' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2022' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2022' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2023' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2023' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2024' },
  { category: 'Juvenile Couple', names: 'Ádám Király & Dorottya Novák', year: '2024' },
];
</script>


<template>
  <section class="relative text-white pt-16">
    <!-- Background Image & Overlay -->
    <div class="absolute inset-0">
      <NuxtImg src="/img/word.webp" alt="Champions background" class="w-full h-full object-cover" />
      <div class="absolute inset-0 bg-black opacity-75"></div>
    </div>
    <div class="absolute inset-0" style="opacity: 0.6; background: #141414;"></div>

    <!-- Main Content -->
    <div class="relative container mx-auto px-4">
      <!-- Heading -->
      <div class="text-center mb-12">
        <h2 class="font-unbounded uppercase text-cyan-400 font-medium text-[20px] lg:text-[32px]">Our World Champions</h2>
        <p class="mt-4 max-w-2xl mx-auto text-lg leading-relaxed text-[16px]">
          Our coaches are living legends of the sport! Their competitive dancers have claimed 26 World Championship titles in the past 13 years under the WRRC
          (World Rock'n'Roll Confederation) – an unprecedented achievement in the history of acrobatic rock 'n' roll!
        </p>
      </div>

      <!-- Cards Grid -->
      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
        <div v-for="(item, index) in champions" :key="index" class="bg-white text-[#242424] rounded-lg shadow-lg flex flex-col pt-8 px-8 min-h-[200px] relative" :class="{ 'lg:col-start-2': index === champions.length - 2, 'lg:col-start-3': index === champions.length - 1 }">
          <div class="text-center flex-grow">
            <h3 class="text-[18px] lg:text-[18px] uppercase font-bold">
              {{ item.category }}
            </h3>
            <p class="mt-2 text-[16px] lg:text-[16px] font-normal uppercase">
              {{ item.names }}
            </p>
          </div>
          <div class="absolute bottom-[-25px] left-0 right-0 text-center pb-0">
            <span class="text-[40px] lg:text-[65px] font-black font-unbounded">
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
          <h3 class="font-unbounded text-[30px] font-extrabold uppercase text-white">Explore our<br />Training Programs</h3>
          <div class="inline-flex items-center mt-6">
            <button class="bg-[#FE2AF7] text-[16px] text-black uppercase font-bold py-4 px-20 shadow-lg">Coming Soon</button>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>