<template>
    <section class="relative z-10 w-full h-[800px] overflow-hidden">
        <!-- Animált háttérkép -->
        <NuxtImg 
            src="/img/hero.svg" 
            format="webp" 
            class="absolute inset-0 object-cover w-full h-full pointer-events-none hero-bg"
        />
        
        <!-- Overlay animációval -->
        <div class="absolute inset-0 overlay" style="background: #141414;"></div>
        
        <!-- Fő tartalom -->
        <div class="container relative z-10 h-full flex items-center justify-center flex-col">
           <div class="text-center text-white">
               <!-- Welcome szöveg -->
               <h3 class="text-lg mb-4 font-unbounded welcome-text opacity-0">
                   Welcome to the world of
               </h3>
               
               <!-- Fő címsor -->
               <h1 class="text-5xl font-bold mb-2 font-unbounded text-[32px] lg:text-[64px] main-title opacity-0">
                   MOVE DANCE
               </h1>
               
               <!-- Alcímsor -->
               <h2 class="text-3xl mb-8 font-unbounded text-[20px] lg:text-[32px] subtitle opacity-0">
                   SPORT ACADEMY
               </h2>
           </div>
        </div>
        
        <!-- Animált logó -->
        <img 
            src="/img/logo.png" 
            alt="Logo" 
            class="absolute mb-10 bottom-0 left-1/2 transform -translate-x-1/2 w-[100px] logo opacity-0" 
        />
    </section>
</template>

<style scoped>
/* Háttérkép animáció - finom zoom és lebegés */
.hero-bg {
    animation: backgroundFloat 20s ease-in-out infinite;
}

@keyframes backgroundFloat {
    0%, 100% {
        transform: scale(1) translateY(0px);
    }
    50% {
        transform: scale(1.05) translateY(-10px);
    }
}

/* Overlay fade-in */
.overlay {
    opacity: 0;
    animation: fadeInOverlay 1s ease-out 0.3s forwards;
}

@keyframes fadeInOverlay {
    to {
        opacity: 0.6;
    }
}

/* Welcome szöveg animáció */
.welcome-text {
    animation: slideInFromTop 1s ease-out 0.8s forwards;
}

/* Fő cím animáció */
.main-title {
    animation: slideInScale 1.2s ease-out 1.2s forwards;
    transform: translateY(30px) scale(0.9);
}

/* Alcím animáció */
.subtitle {
    animation: slideInFromBottom 1s ease-out 1.6s forwards;
    transform: translateY(50px);
}

/* Logó animáció */
.logo {
    animation: bounceInLogo 1.5s cubic-bezier(0.68, -0.55, 0.265, 1.55) 2s forwards;
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
    0%, 100% { text-shadow: 0 0 5px rgba(255, 255, 255, 0.5); }
    50% { text-shadow: 0 0 20px rgba(255, 255, 255, 0.8); }
}

@keyframes titlePulse {
    0%, 100% { transform: scale(1); }
    50% { transform: scale(1.05); }
}

@keyframes subtitleShake {
    0%, 100% { transform: translateX(0); }
    25% { transform: translateX(-3px); }
    75% { transform: translateX(3px); }
}

/* Responsive animációk */
@media (max-width: 1024px) {
    .main-title {
        animation-delay: 1s;
    }
    
    .subtitle {
        animation-delay: 1.4s;
    }
    
    .logo {
        animation-delay: 1.8s;
    }
}

/* Prefers-reduced-motion támogatás */
@media (prefers-reduced-motion: reduce) {
    .hero-bg,
    .welcome-text,
    .main-title,
    .subtitle,
    .logo {
        animation: none;
        opacity: 1;
        transform: none;
    }
    
    .overlay {
        opacity: 0.6;
    }
    
    .logo {
        transform: translateX(-50%);
    }
}

/* További finomhangolások */
.main-title {
    letter-spacing: 2px;
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
}

.subtitle {
    letter-spacing: 1px;
    text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.5);
}

.logo {
    filter: drop-shadow(0 4px 8px rgba(0, 0, 0, 0.3));
    transition: filter 0.3s ease;
}

.logo:hover {
    filter: drop-shadow(0 6px 12px rgba(255, 255, 255, 0.2));
}
</style>