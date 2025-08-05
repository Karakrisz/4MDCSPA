<script setup>
const competitions = [
  { title: 'International Competition', location: 'Soroksár Sports Hall', date: 'Sat, 13 Dec' },
  { title: 'International Competition', location: 'Soroksár Sports Hall', date: 'Sat, 13 Dec' },
];
</script>

<template>
  <section class="relative text-white">
    <!-- Background image -->
    <div class="absolute inset-0">
      <NuxtImg src="/img/about.svg" alt="Competitions background" class="w-full h-full object-cover" />
      <div class="absolute inset-0 bg-black opacity-60"></div>
    </div>

    <div class="relative container mx-auto px-4 py-24 flex flex-col items-center">
      <!-- Title -->
      <h2 class="text-4xl md:text-5xl font-extrabold uppercase mb-12 text-center text-white">Join the Competitions</h2>

      <!-- Competitions Box -->
      <div class="bg-white rounded-lg shadow-lg w-full max-w-2xl p-8">
        <div v-for="(comp, idx) in competitions" :key="idx" class="flex items-center py-4 border-b last:border-0">
          <div class="w-20 h-20 bg-gray-200 rounded mr-6 flex-shrink-0"></div>
          <div>
            <h3 class="text-xl font-bold text-[#242424]">
              {{ comp.title }}
            </h3>
            <p class="mt-1 text-[#242424]">
              {{ comp.location }}
            </p>
            <p class="mt-1 font-semibold text-[#242424]">
              {{ comp.date }}
            </p>
          </div>
        </div>

        <!-- Show All Button -->
        <div class="mt-8 text-center">
          <button class="bg-[#242424] text-white uppercase font-bold py-4 px-10 rounded">Show All</button>
        </div>
      </div>
    </div>
  </section>
</template>
