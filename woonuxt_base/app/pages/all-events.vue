<script setup>
// Összes esemény adatok
const allEvents = [
  { 
    title: 'Grand Opening - First Open Training', 
    location: 'Dubai, UAE', 
    date: 'Fri, 30 Aug',
    type: 'Grand Opening',
    status: 'active'
  },
  { 
    title: 'GEMS FirstPoint School - Sign-up day', 
    location: 'GEMS FirstPoint School', 
    date: 'Sun, 1 Sept',
    type: 'Sign-up',
    status: 'active'
  },
  { 
    title: 'Raffles World Academy - First AACA training', 
    location: 'Raffles World Academy', 
    date: 'Sun, 8 Sept',
    type: 'Training',
    status: 'upcoming'
  },
  { 
    title: 'GEMS FirstPoint School - ASA First training', 
    location: 'GEMS FirstPoint School', 
    date: 'Mon, 9 Sept',
    type: 'Training',
    status: 'upcoming',
    registration: 'https://esm.ae/login'
  },
  { 
    title: 'Raffles International School - First CCA training', 
    location: 'Raffles International School', 
    date: 'Wed, 11 Sept',
    type: 'Training',
    status: 'upcoming'
  },
  { 
    title: 'GEMS World Academy - First Academic training', 
    location: 'GEMS World Academy', 
    date: 'Sun, 15 Sept',
    type: 'Training',
    status: 'upcoming'
  },
  { 
    title: 'Deira International School - First Academic training', 
    location: 'Deira International School', 
    date: 'Sun, 15 Sept',
    type: 'Training',
    status: 'upcoming',
    note: 'ECA Coming Soon'
  },
  { 
    title: 'GEMS Metropole School - Motor city - First Academic training', 
    location: 'GEMS Metropole School - Motor city', 
    date: 'Mon, 16 Sept',
    type: 'Training',
    status: 'upcoming',
    note: 'ASA Coming Soon'
  },
  { 
    title: 'Dubai Schools - Al Khawaneej - First Academic training', 
    location: 'Dubai Schools - Al Khawaneej', 
    date: 'Mon, 16 Sept',
    type: 'Training',
    status: 'upcoming',
    note: 'ECA Coming Soon'
  },
  { 
    title: 'Dubai International Academy - Al Barsha - First CCA training', 
    location: 'Dubai International Academy - Al Barsha', 
    date: 'Wed, 18 Sept',
    type: 'Training',
    status: 'upcoming'
  },
  { 
    title: 'GEMS Metropole School- Al Waha - First Academic training', 
    location: 'GEMS Metropole School- Al Waha', 
    date: 'Thu, 19 Sept',
    type: 'Training',
    status: 'upcoming',
    note: 'ASA Coming Soon'
  },
  { 
    title: 'First national competition - Move Dance Contest in the UAE', 
    location: 'UAE', 
    date: 'Spring 2026',
    type: 'Competition',
    status: 'planned'
  }
];

import { ref, onMounted, onUnmounted } from 'vue'

const eventsContainer = ref(null)
const filteredEvents = ref([...allEvents]) // Másolat készítése
const activeFilter = ref('all')

// Javított szűrési funkciók
const filterEvents = (filterType) => {
  console.log('Filtering by:', filterType) // Debug
  activeFilter.value = filterType
  
  if (filterType === 'all') {
    filteredEvents.value = [...allEvents] // Új másolat készítése
  } else if (filterType === 'training') {
    filteredEvents.value = allEvents.filter(event => event.type === 'Training')
  } else if (filterType === 'sign-up') {
    filteredEvents.value = allEvents.filter(event => event.type === 'Sign-up')
  } else if (filterType === 'competition') {
    filteredEvents.value = allEvents.filter(event => event.type === 'Competition')
  } else if (filterType === 'grand opening') {
    filteredEvents.value = allEvents.filter(event => event.type === 'Grand Opening')
  }
  
  console.log('Filtered events count:', filteredEvents.value.length) // Debug
  
  // Újraanimálás a szűrés után
  setTimeout(() => {
    const items = eventsContainer.value?.querySelectorAll('.event-card')
    items?.forEach((item, index) => {
      item.classList.remove('animate-in')
      item.style.setProperty('--item-delay', `${index * 0.1}s`)
      setTimeout(() => {
        item.classList.add('animate-in')
      }, index * 50)
    })
  }, 50)
}

// Visszalépés a főoldalra
const goBack = () => {
  // Itt a router back-et használnád: $router.go(-1) vagy $router.push('/')
  console.log('Go back to main page')
}

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
      rootMargin: '0px 0px -100px 0px'
    }
  )

  // Animálandó elemek megfigyelése
  const elementsToAnimate = eventsContainer.value?.querySelectorAll('.event-card, .page-title, .filter-buttons')
  elementsToAnimate?.forEach((el, index) => {
    if (el.classList.contains('event-card')) {
      el.style.setProperty('--item-delay', `${index * 0.1}s`)
    }
    observer.observe(el)
  })

  onUnmounted(() => {
    observer.disconnect()
  })
})
</script>

<template>
  <section class="relative text-white min-h-screen">
    <!-- Background image ugyanaz, mint a főoldalon -->
    <div class="absolute inset-0 bg-container">
      <NuxtImg 
        src="/img/com.webp" 
        alt="Events background" 
        class="w-full h-full object-cover bg-image" 
      />
      <div class="absolute inset-0 bg-black opacity-70 bg-overlay"></div>
    </div>

    <div class="relative container mx-auto px-4 py-16">
      <!-- Header -->
      <div class="flex items-center justify-between mt-[100px] mb-12">
        <button 
          @click="goBack"
          class="flex items-center space-x-2 text-white/80 hover:text-white transition-colors duration-300 group"
        >
          <svg class="w-6 h-6 transform group-hover:-translate-x-1 transition-transform duration-300" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7"></path>
          </svg>
          <span class="text-lg font-semibold">Back</span>
        </button>
        
        <h1 class="page-title text-[32px] lg:text-[48px] font-extrabold font-unbounded text-center bg-gradient-to-r from-[#00BCD4] to-[#FE2AF7] bg-clip-text text-transparent">
          ALL EVENTS
        </h1>
        
        <div class="w-20"></div> <!-- Spacer for centering -->
      </div>

      <!-- Filter Buttons -->
      <div class="filter-buttons flex flex-wrap justify-center gap-4 mb-12">
        <button 
          @click="filterEvents('all')"
          :class="activeFilter === 'all' ? 'filter-btn-active' : 'filter-btn-inactive'"
        >
          All Events ({{ allEvents.length }})
        </button>
        <button 
          @click="filterEvents('training')"
          :class="activeFilter === 'training' ? 'filter-btn-active' : 'filter-btn-inactive'"
        >
          Trainings ({{ allEvents.filter(e => e.type === 'Training').length }})
        </button>
        <button 
          @click="filterEvents('sign-up')"
          :class="activeFilter === 'sign-up' ? 'filter-btn-active' : 'filter-btn-inactive'"
        >
          Sign-ups ({{ allEvents.filter(e => e.type === 'Sign-up').length }})
        </button>
        <button 
          @click="filterEvents('competition')"
          :class="activeFilter === 'competition' ? 'filter-btn-active' : 'filter-btn-inactive'"
        >
          Competitions ({{ allEvents.filter(e => e.type === 'Competition').length }})
        </button>
      </div>

      <!-- Events Grid -->
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6" ref="eventsContainer">
        <div 
          v-for="(event, index) in filteredEvents" 
          :key="`${event.title}-${index}`"
          class="event-card opacity-0 transform translate-y-10"
          :style="{ transitionDelay: `var(--item-delay, 0s)` }"
        >
          <div class="bg-white rounded-lg shadow-lg p-6 h-full hover:shadow-2xl transition-all duration-300 hover:transform hover:scale-105 group relative overflow-hidden">
            <!-- Status Badge -->
            <div class="absolute top-4 right-4">
              <span :class="[
                'px-3 py-1 rounded-full text-xs font-bold uppercase tracking-wide',
                event.status === 'active' ? 'bg-green-500 text-white' :
                event.status === 'upcoming' ? 'bg-blue-500 text-white' :
                'bg-purple-500 text-white'
              ]">
                {{ event.status }}
              </span>
            </div>

            <!-- Event Icon -->
            <div class="w-16 h-16 bg-gray-200 rounded mr-0 mb-4 flex-shrink-0 competition-icon group-hover:scale-110 transition-transform duration-300">
              <div class="w-full h-full rounded bg-gradient-to-br from-gray-300 to-gray-400 flex items-center justify-center icon-content">
                <div v-if="event.type === 'Grand Opening'" class="w-8 h-8 border-4 border-[#00BCD4] rounded-full animate-pulse"></div>
                <div v-else-if="event.type === 'Sign-up'" class="w-6 h-6 bg-[#FE2AF7] rounded"></div>
                <div v-else-if="event.type === 'Competition'" class="w-8 h-8">
                  <svg fill="#242424" viewBox="0 0 24 24">
                    <path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-2 15l-5-5 1.41-1.41L10 14.17l7.59-7.59L19 8l-9 9z"/>
                  </svg>
                </div>
                <div v-else class="w-8 h-8 border-4 border-[#242424] rounded-full animate-pulse"></div>
              </div>
            </div>

            <!-- Event Details -->
            <div class="space-y-3">
              <h3 class="text-xl font-extrabold text-[#242424] font-unbounded group-hover:text-[#00BCD4] transition-colors duration-300 leading-tight">
                {{ event.title }}
              </h3>
              
              <p class="text-[#242424]/70 flex items-center space-x-2">
                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z"/>
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 11a3 3 0 11-6 0 3 3 0 016 0z"/>
                </svg>
                <span>{{ event.location }}</span>
              </p>
              
              <p class="text-[#242424] font-semibold flex items-center space-x-2">
                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"/>
                </svg>
                <span>{{ event.date }}</span>
              </p>

              <!-- Additional Info -->
              <div v-if="event.note" class="mt-4 p-3 bg-yellow-100 rounded-lg border border-yellow-300">
                <p class="text-yellow-800 text-sm font-medium flex items-center space-x-2">
                  <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"/>
                  </svg>
                  <span>{{ event.note }}</span>
                </p>
              </div>

              <!-- Registration Link -->
              <!-- <div v-if="event.registration" class="mt-4">
                <a 
                  :href="event.registration" 
                  target="_blank"
                  class="inline-flex items-center space-x-2 bg-[#242424] text-white px-6 py-3 rounded font-semibold text-sm hover:bg-gradient-to-r hover:from-[#00BCD4] hover:to-[#FE2AF7] transition-all duration-300 hover:scale-105 hover:shadow-lg"
                >
                  <span>Register Now</span>
                  <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14"/>
                  </svg>
                </a>
              </div> -->
            </div>

            <!-- Hover Effect Border -->
            <div class="absolute inset-0 border-2 border-transparent group-hover:border-[#00BCD4] rounded-lg transition-colors duration-300"></div>
          </div>
        </div>
      </div>

      <!-- Empty State -->
      <div v-if="filteredEvents.length === 0" class="text-center py-16">
        <div class="w-24 h-24 mx-auto mb-6 bg-white/10 backdrop-blur-sm rounded-full flex items-center justify-center">
          <svg class="w-12 h-12 text-white/50" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"/>
          </svg>
        </div>
        <h3 class="text-2xl font-bold text-white mb-2">No Events Found</h3>
        <p class="text-white/70">Try selecting a different filter or check back later for new events.</p>
      </div>

      <!-- Footer Info -->
      <div class="mt-16 text-center">
        <div class="bg-white/10 backdrop-blur-sm rounded-lg p-8 border border-white/20">
          <p class="text-white/70 mb-4 text-lg">
            Stay updated with our latest events and training sessions
          </p>
          <div class="flex flex-wrap justify-center gap-6">
            <div class="flex items-center space-x-2 text-[#00BCD4]">
              <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"/>
              </svg>
              <span class="font-semibold">{{ allEvents.length }} Total Events</span>
            </div>
            <div class="flex items-center space-x-2 text-[#FE2AF7]">
              <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z"/>
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 11a3 3 0 11-6 0 3 3 0 016 0z"/>
              </svg>
              <span class="font-semibold">Multiple Locations</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
/* Háttér animációk - ugyanaz, mint a főoldalon */
.bg-container {
  overflow: hidden;
}

.bg-image {
  transform: scale(1.1);
  animation: backgroundDrift 25s ease-in-out infinite;
}

@keyframes backgroundDrift {
  0%, 100% {
    transform: scale(1.1) translateX(0px) translateY(0px);
  }
  25% {
    transform: scale(1.15) translateX(-20px) translateY(-10px);
  }
  50% {
    transform: scale(1.12) translateX(15px) translateY(-5px);
  }
  75% {
    transform: scale(1.18) translateX(-10px) translateY(10px);
  }
}

.bg-overlay {
  animation: overlayBreathe 6s ease-in-out infinite;
}

@keyframes overlayBreathe {
  0%, 100% { opacity: 0.7; }
  50% { opacity: 0.5; }
}

/* Page title animáció */
.page-title {
  transform: translateY(60px) scale(0.8);
  transition: all 1.2s cubic-bezier(0.34, 1.56, 0.64, 1);
  background: linear-gradient(45deg, #ffffff, #00BCD4, #FE2AF7, #ffffff);
  background-size: 300% 100%;
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  animation: titleGradient 3s ease-in-out infinite;
}

@keyframes titleGradient {
  0%, 100% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
}

.page-title.animate-in {
  opacity: 1;
  transform: translateY(0) scale(1);
}

/* Filter buttons CSS */
.filter-btn-inactive {
  padding: 1rem 2rem;
  border-radius: 0.5rem;
  font-weight: bold;
  font-size: 0.875rem;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  transition: all 0.3s ease;
  background-color: white;
  color: #242424;
  border: 2px solid white;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.filter-btn-inactive:hover {
  background-color: #f3f4f6;
  box-shadow: 0 10px 15px rgba(0, 0, 0, 0.2);
  transform: translateY(-2px);
}

.filter-btn-active {
  padding: 1rem 2rem;
  border-radius: 0.5rem;
  font-weight: bold;
  font-size: 0.875rem;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  transition: all 0.3s ease;
  background: linear-gradient(45deg, #00BCD4, #FE2AF7);
  color: white;
  border: 2px solid transparent;
  box-shadow: 0 10px 20px rgba(0, 188, 212, 0.3);
  transform: scale(1.05);
}

.filter-btn-active:hover {
  box-shadow: 0 15px 25px rgba(0, 188, 212, 0.4);
  transform: scale(1.08);
}

/* Filter buttons animáció */
.filter-buttons {
  transform: translateY(40px);
  transition: all 1s cubic-bezier(0.25, 0.46, 0.45, 0.94) 0.3s;
}

.filter-buttons.animate-in {
  opacity: 1;
  transform: translateY(0);
}

/* Event card animációk */
.event-card {
  transition: all 0.8s cubic-bezier(0.25, 0.46, 0.45, 0.94) calc(0.6s + var(--item-delay, 0s));
}

.event-card.animate-in {
  opacity: 1;
  transform: translateY(0);
}

/* Icon animációk - ugyanaz, mint a főoldalon */
.competition-icon {
  transform: scale(0) rotate(-180deg);
  transition: all 0.8s cubic-bezier(0.34, 1.56, 0.64, 1) calc(0.8s + var(--item-delay, 0s));
}

.event-card.animate-in .competition-icon {
  transform: scale(1) rotate(0deg);
}

.icon-content {
  position: relative;
  overflow: hidden;
}

.icon-content::after {
  content: '';
  position: absolute;
  top: -50%;
  left: -50%;
  width: 200%;
  height: 200%;
  background: linear-gradient(45deg, transparent, rgba(0, 188, 212, 0.3), transparent);
  transform: rotate(45deg);
  animation: iconShimmer 2s ease-in-out infinite;
}

@keyframes iconShimmer {
  0% { transform: translateX(-100%) translateY(-100%) rotate(45deg); }
  100% { transform: translateX(100%) translateY(100%) rotate(45deg); }
}

/* Responsive */
@media (max-width: 768px) {
  .page-title {
    font-size: 24px;
  }
  
  .filter-buttons {
    gap: 2rem;
  }
  
  .filter-buttons button {
    padding: 0.75rem 1.5rem;
    font-size: 0.75rem;
  }
}

/* Accessibility - prefers-reduced-motion */
@media (prefers-reduced-motion: reduce) {
  .bg-image,
  .bg-overlay {
    animation: none;
  }
  
  .page-title {
    animation: none;
    background: linear-gradient(45deg, #00BCD4, #FE2AF7);
    background-clip: text;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
  }
  
  .page-title,
  .filter-buttons,
  .event-card {
    opacity: 1;
    transform: none;
    transition: none;
  }
  
  .competition-icon {
    transform: scale(1) rotate(0deg);
  }
  
  .icon-content::after {
    display: none;
  }
}
</style>