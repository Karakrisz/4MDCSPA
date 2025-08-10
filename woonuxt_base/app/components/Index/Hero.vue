<template>
    <section class="relative z-10 w-full h-[800px] overflow-hidden">
        <NuxtImg src="/img/hero.svg" format="webp" class="absolute inset-0 object-cover w-full h-full pointer-events-none"/>
        <div class="absolute inset-0" style="opacity: 0.6; background: #141414;"></div>
        <div class="container relative z-10 h-full flex items-center justify-center flex-col">
           <div class="text-center text-white">
               <h3 class="text-lg mb-4 font-unbounded">Welcome to the world of</h3>
               <h1 class="text-5xl font-bold mb-2 font-unbounded text-[32px] lg:text-[64px]">MOVE DANCE</h1>
               <h2 class="text-3xl mb-8 font-unbounded text-[20px] lg:text-[32px]">SPORT ACADEMY</h2>
           </div>
        </div>
        <img src="/img/logo.png" alt="Logo" class="absolute mb-10 bottom-0 left-1/2 transform -translate-x-1/2 w-[100px]" />
    </section>
</template>