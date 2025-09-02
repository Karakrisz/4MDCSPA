<script setup>
import { ref, onMounted, computed, nextTick, onUnmounted } from 'vue';

const mapContainer = ref(null);
const map = ref(null);
const loading = ref(true);
const showInfo = ref(true); // Mindig látható legyen a panel
const panelCollapsed = ref(false); // Panel összecsukott állapota mobil nézetben
const activeFilter = ref('all');
const markers = ref([]);

// Location data
const locations = ref([
  // Academies
  {
    id: 1,
    name: 'Deira International School',
    address: 'Happiness Center',
    city: 'Dubai',
    type: 'academy',
    schedule: 'Monday & Wednesday 17:00-18:00 | Start: 2025.09.15',
    coordinates: [25.2582, 55.3311],
  },
  {
    id: 2,
    name: 'Dubai Schools Al Khawaneej',
    address: 'X1 KG GYM',
    city: 'Dubai',
    type: 'academy',
    schedule: 'Tuesday & Thursday 16:30-17:30 | Start: 2025.09.16',
    coordinates: [25.2631, 55.4041],
  },
  {
    id: 3,
    name: 'Gems Metropol School Al waha',
    address: 'FS Indoor Play Area 2',
    city: 'Dubai',
    type: 'academy',
    schedule: 'Friday 13:30-14:30 | Start: 2025.09.19 or 09.12',
    coordinates: [25.1194, 55.1926],
  },
  {
    id: 4,
    name: 'Gems Metropol School',
    address: 'Dance Studio',
    city: 'Dubai',
    type: 'academy',
    schedule: 'Monday & Wednesday 17:00-18:00 | Start: 2025.09.15',
    coordinates: [25.0657, 55.1713],
  },
  {
    id: 5,
    name: 'Gems World Academy',
    address: 'Junior Gym',
    city: 'Dubai',
    type: 'academy',
    schedule: 'Tuesday & Thursday 17:00-18:00 | Start: 2025.09.16',
    coordinates: [25.0407, 55.1719],
  },

  // Dubai Afterschool Activities
  {
    id: 10,
    name: 'DIA Al Barsha',
    address: 'Al Barsha',
    city: 'Dubai',
    type: 'afterschool',
    coordinates: [25.1112, 55.1994],
  },
  {
    id: 11,
    name: 'Raffles World Academy',
    address: 'Dubai',
    city: 'Dubai',
    type: 'afterschool',
    coordinates: [25.0407, 55.1719],
  },
  {
    id: 12,
    name: 'Raffles International Academy',
    address: 'Dubai',
    city: 'Dubai',
    type: 'afterschool',
    coordinates: [25.0501, 55.1838],
  },
  {
    id: 13,
    name: 'Collegiate International School',
    address: 'Dubai',
    city: 'Dubai',
    type: 'afterschool',
    coordinates: [25.078, 55.1562],
  },
  {
    id: 14,
    name: 'Gems International School',
    address: 'Dubai',
    city: 'Dubai',
    type: 'afterschool',
    coordinates: [25.0941, 55.156],
  },
  {
    id: 15,
    name: 'Gems Wellington Academy',
    address: 'Dubai',
    city: 'Dubai',
    type: 'afterschool',
    coordinates: [25.0501, 55.1838],
  },
  {
    id: 16,
    name: 'Gems Metropol School Al waha',
    address: 'Dubai',
    city: 'Dubai',
    type: 'afterschool',
    coordinates: [25.1194, 55.1926],
  },
  {
    id: 17,
    name: 'Gems Metropol School',
    address: 'Dubai',
    city: 'Dubai',
    type: 'afterschool',
    coordinates: [25.0657, 55.1713],
  },
  {
    id: 18,
    name: 'Gems World Academy',
    address: 'Dubai',
    city: 'Dubai',
    type: 'afterschool',
    coordinates: [25.0407, 55.1719],
  },
  {
    id: 19,
    name: 'Gems First Point School',
    address: 'Dubai',
    city: 'Dubai',
    type: 'afterschool',
    coordinates: [25.0315, 55.1713],
  },

  // Abu Dhabi Afterschool Activities
  {
    id: 20,
    name: 'GEMS World Academy',
    address: 'Abu Dhabi',
    city: 'Abu Dhabi',
    type: 'afterschool',
    coordinates: [24.4539, 54.3773],
  },
  {
    id: 21,
    name: 'Gems American Academy',
    address: 'Abu Dhabi',
    city: 'Abu Dhabi',
    type: 'afterschool',
    coordinates: [24.4682, 54.3811],
  },
]);

const filteredLocations = computed(() => {
  if (activeFilter.value === 'all') return locations.value;
  return locations.value.filter((location) => location.type === activeFilter.value);
});

const academyCount = computed(() => locations.value.filter((l) => l.type === 'academy').length);

const afterschoolCount = computed(() => locations.value.filter((l) => l.type === 'afterschool').length);

// Check if we're on desktop
const isDesktop = () => window.innerWidth >= 1024;

const loadLeaflet = () => {
  return new Promise((resolve) => {
    if (window.L) {
      resolve();
      return;
    }

    // Load CSS
    const css = document.createElement('link');
    css.rel = 'stylesheet';
    css.href = 'https://unpkg.com/leaflet@1.9.4/dist/leaflet.css';
    css.integrity = 'sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY=';
    css.crossOrigin = '';
    document.head.appendChild(css);

    // Load JS
    const script = document.createElement('script');
    script.src = 'https://unpkg.com/leaflet@1.9.4/dist/leaflet.js';
    script.integrity = 'sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo=';
    script.crossOrigin = '';
    script.onload = resolve;
    document.head.appendChild(script);
  });
};

const initMap = async () => {
  try {
    await loadLeaflet();

    // Initialize map centered on Dubai
    map.value = L.map(mapContainer.value, {
      scrollWheelZoom: false, // Disable scroll wheel zoom by default
      doubleClickZoom: true,
      dragging: true,
      zoomControl: true,
      touchZoom: true,
    }).setView([25.2048, 55.2708], 10);

    // Add OpenStreetMap tiles
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      maxZoom: 19,
      attribution: '© OpenStreetMap contributors',
    }).addTo(map.value);

    // Enable scroll wheel zoom only when map is focused
    map.value.on('focus', () => {
      map.value.scrollWheelZoom.enable();
    });

    map.value.on('blur', () => {
      map.value.scrollWheelZoom.disable();
    });

    // Enable scroll wheel zoom on mouse enter, disable on mouse leave
    mapContainer.value.addEventListener('mouseenter', () => {
      map.value.scrollWheelZoom.enable();
    });

    mapContainer.value.addEventListener('mouseleave', () => {
      map.value.scrollWheelZoom.disable();
    });

    // Add click event listener for popup buttons
    map.value.on('popupopen', () => {
      nextTick(() => {
        const buttons = document.querySelectorAll('.popup-directions-btn');
        buttons.forEach((button) => {
          button.addEventListener('click', (e) => {
            const lat = e.target.dataset.lat;
            const lng = e.target.dataset.lng;
            openDirectionsFromCoords(lat, lng);
          });
        });
      });
    });

    // Add all markers
    addMarkers();

    // Set initial showInfo state - always visible
    showInfo.value = true;

    loading.value = false;
  } catch (error) {
    console.error('Error loading map:', error);
    loading.value = false;
  }
};

const addMarkers = () => {
  // Clear existing markers
  markers.value.forEach((marker) => map.value.removeLayer(marker));
  markers.value = [];

  filteredLocations.value.forEach((location) => {
    const icon = L.divIcon({
      html: `
        <div class="flex items-center justify-center w-12 h-12 rounded-full border-4 border-white shadow-2xl ${
          location.type === 'academy' ? 'bg-[#FE2AF7]' : 'bg-[#24d3ee]'
        } hover:scale-110 transition-all duration-300 animate-pulse">
          <svg class="w-6 h-6 text-white drop-shadow-lg" fill="currentColor" viewBox="0 0 20 20">
            <path fill-rule="evenodd" d="M5.05 4.05a7 7 0 119.9 9.9L10 18.9l-4.95-4.95a7 7 0 010-9.9zM10 11a2 2 0 100-4 2 2 0 000 4z" clip-rule="evenodd" />
          </svg>
        </div>
      `,
      className: 'custom-div-icon',
      iconSize: [48, 48],
      iconAnchor: [24, 24],
    });

    const marker = L.marker(location.coordinates, { icon }).addTo(map.value).bindPopup(`
        <div class="p-4 min-w-[250px] bg-gray-900 rounded-lg border border-gray-700">
          <h3 class="font-bold  mb-2 text-lg">${location.name}</h3>
          <p class="text-sm text-gray-400 mb-3">${location.address}</p>
          <div class="flex gap-2 mb-3">
            <span class="px-3 py-1 rounded-full text-xs font-bold text-white shadow-lg ${
              location.type === 'academy' ? 'bg-[#FE2AF7] shadow-[#FE2AF7]/30' : 'bg-[#24d3ee] shadow-[#24d3ee]/30'
            }">
              ${location.type === 'academy' ? 'Academy' : 'Afterschool'}
            </span>
            <span class="px-3 py-1 rounded-full text-xs font-bold text-white shadow-lg ${
              location.city === 'Dubai' ? 'bg-blue-500 shadow-blue-500/30' : 'bg-orange-500 shadow-orange-500/30'
            }">
              ${location.city}
            </span>
          </div>
          ${location.schedule ? `<p class="text-xs text-gray-400 mb-4">${location.schedule}</p>` : ''}
          <button 
            class="w-full bg-gradient-to-r from-[#FE2AF7] to-[#24d3ee] text-white px-4 py-2 rounded-xl text-sm font-bold hover:shadow-lg hover:shadow-[#FE2AF7]/30 transition-all duration-300 popup-directions-btn"
            data-lat="${location.coordinates[0]}" 
            data-lng="${location.coordinates[1]}"
          >
            Get Directions
          </button>
        </div>
      `);

    markers.value.push(marker);
  });
};

const setFilter = (filter) => {
  activeFilter.value = filter;
  nextTick(() => {
    addMarkers();
    if (filteredLocations.value.length > 0) {
      fitAllMarkers();
    }
  });
};

const focusOnLocation = (location) => {
  if (map.value) {
    map.value.setView(location.coordinates, 16);

    // Mobile-on összecsukjuk a panelt, hogy jobban lássa a térképet
    if (!isDesktop()) {
      panelCollapsed.value = true;
    }

    // Find and open the popup for this location
    const marker = markers.value.find((m) => m.getLatLng().lat === location.coordinates[0] && m.getLatLng().lng === location.coordinates[1]);
    if (marker) {
      marker.openPopup();
    }
  }
};

const openDirections = (location) => {
  const url = `https://www.google.com/maps/dir/?api=1&destination=${location.coordinates[0]},${location.coordinates[1]}`;
  window.open(url, '_blank');
};

const openDirectionsFromCoords = (lat, lng) => {
  const url = `https://www.google.com/maps/dir/?api=1&destination=${lat},${lng}`;
  window.open(url, '_blank');
};

const getUserLocation = () => {
  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition(
      (position) => {
        const userLocation = [position.coords.latitude, position.coords.longitude];

        if (map.value) {
          map.value.setView(userLocation, 15);

          // Add user location marker
          const userIcon = L.divIcon({
            html: `
              <div class="flex items-center justify-center w-10 h-10 rounded-full bg-gradient-to-br from-blue-400 to-indigo-600 border-4 border-white shadow-2xl animate-pulse">
                <div class="w-4 h-4 bg-white rounded-full shadow-sm"></div>
              </div>
            `,
            className: 'user-location-icon',
            iconSize: [40, 40],
            iconAnchor: [20, 20],
          });

          L.marker(userLocation, { icon: userIcon }).addTo(map.value).bindPopup('Your current location').openPopup();
        }
      },
      (error) => {
        console.error('Error getting location:', error);
        alert('Unable to determine your current location.');
      },
    );
  } else {
    alert('Your browser does not support location services.');
  }
};

const fitAllMarkers = () => {
  if (map.value && markers.value.length > 0) {
    const group = new L.featureGroup(markers.value);
    map.value.fitBounds(group.getBounds().pad(0.1));
  }
};

// Handle window resize - no need to change panel visibility
// const handleResize = () => {
//   if (isDesktop() && !showInfo.value) {
//     showInfo.value = true
//   }
// }

onMounted(() => {
  nextTick(() => {
    initMap();
    // window.addEventListener('resize', handleResize)
  });
});

onUnmounted(() => {
  if (map.value) {
    map.value.remove();
  }
  // window.removeEventListener('resize', handleResize)
});
</script>

<template>
  <div class="w-full h-screen relative overflow-hidden flex flex-col" style="background: linear-gradient(180deg, rgba(0, 0, 0, 0) 55.37%, #000 100%)">
    <!-- Loading state -->
    <div v-if="loading" class="absolute inset-0 flex items-center justify-center bg-black/80 backdrop-blur-sm z-50">
      <div class="flex flex-col items-center">
        <div class="animate-spin rounded-full h-12 w-12 border-b-2 border-[#FE2AF7]"></div>
        <p class="mt-4 text-white">Loading map...</p>
      </div>
    </div>

    <!-- Map container - fixed height -->
    <div class="flex-1 relative">
      <div ref="mapContainer" class="w-full h-full"></div>

      <!-- Map controls (top right) -->
      <div class="absolute top-4 right-4 z-30 flex flex-col gap-2">
        <button
          @click="getUserLocation"
          class="bg-black/80 backdrop-blur-sm border border-gray-800 rounded-xl shadow-2xl p-3 hover:bg-black/90 transition-all duration-300 hover:scale-105 hover:border-[#24d3ee]/50"
          title="My Location">
          <svg class="w-5 h-5 text-[#24d3ee]" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z"></path>
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 11a3 3 0 11-6 0 3 3 0 016 0z"></path>
          </svg>
        </button>
        <button
          @click="fitAllMarkers"
          class="bg-black/80 backdrop-blur-sm border border-gray-800 rounded-xl shadow-2xl p-3 hover:bg-black/90 transition-all duration-300 hover:scale-105 hover:border-white/30"
          title="Show All Locations">
          <svg class="w-5 h-5 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M4 8V4m0 0h4M4 4l5 5m11-1V4m0 0h-4m4 0l-5 5M4 16v4m0 0h4m-4 0l5-5m11 5l-5-5m5 5v-4m0 4h-4"></path>
          </svg>
        </button>

        <!-- Mobile panel toggle button - only show when panel is closed -->
        <button
          v-if="!showInfo"
          @click="showInfo = true"
          class="lg:hidden bg-black/80 backdrop-blur-sm border border-gray-800 rounded-xl shadow-2xl p-3 hover:bg-black/90 transition-all duration-300 hover:scale-105 hover:border-[#FE2AF7]/50"
          title="Show Locations">
          <svg class="w-5 h-5 text-[#FE2AF7]" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 10h16M4 14h16M4 18h16"></path>
          </svg>
        </button>
      </div>
    </div>

    <!-- Bottom panel - fixed height section -->
    <div class="flex-shrink-0 z-30">
      <!-- Panel toggle for mobile (when collapsed) -->
      <div v-if="panelCollapsed" class="flex justify-center mb-2 lg:hidden">
        <button
          @click="panelCollapsed = false"
          class="bg-black/90 backdrop-blur-sm border border-gray-800 rounded-t-xl shadow-2xl px-6 py-3 hover:bg-black transition-all duration-300 flex items-center gap-2">
          <svg class="w-5 h-5 text-[#FE2AF7]" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 15l7-7 7 7"></path>
          </svg>
          <span class="text-white text-sm font-medium">Show Locations</span>
        </button>
      </div>

      <!-- Info panel -->
      <div
        :class="[
          'bg-black/95 backdrop-blur-sm rounded-t-2xl shadow-2xl transition-transform duration-300 border-t border-gray-800',
          panelCollapsed ? 'translate-y-[calc(100%-80px)] lg:translate-y-0' : 'translate-y-0',
        ]">
        <!-- Panel header -->
        <div class="p-6 border-b border-gray-800">
          <div class="flex justify-between items-center mb-4">
            <h3 class="text-2xl font-bold bg-gradient-to-r from-[#FE2AF7] to-[#24d3ee] bg-clip-text text-transparent">Locations</h3>
            <div class="flex items-center gap-3">
              <div class="text-sm text-gray-400">{{ filteredLocations.length }} locations found</div>
              <!-- Collapse button - only on mobile -->
              <button
                @click="panelCollapsed = true"
                class="lg:hidden bg-gray-800/50 hover:bg-gray-700/50 rounded-full p-2 transition-all duration-300"
                title="Minimize">
                <svg class="w-5 h-5 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path>
                </svg>
              </button>
            </div>
          </div>

          <!-- Filter buttons -->
          <div v-if="!panelCollapsed" class="flex flex-wrap gap-3">
            <button
              @click="setFilter('all')"
              :class="[
                'px-6 py-2 rounded-full text-sm font-semibold transition-all duration-300 transform hover:scale-105 border-2',
                activeFilter === 'all'
                  ? 'bg-gradient-to-r from-[#FE2AF7] to-[#24d3ee] text-white shadow-lg border-transparent'
                  : 'bg-gray-800/80 text-white hover:bg-gray-700/90 border-gray-600 hover:border-gray-500 shadow-md',
              ]">
              All ({{ locations.length }})
            </button>
            <button
              @click="setFilter('academy')"
              :class="[
                'px-6 py-2 rounded-full text-sm font-semibold transition-all duration-300 transform hover:scale-105 border-2',
                activeFilter === 'academy'
                  ? 'bg-[#FE2AF7] text-white shadow-lg border-transparent'
                  : 'bg-gray-800/80 text-white hover:bg-[#FE2AF7]/20 border-[#FE2AF7]/50 hover:border-[#FE2AF7] shadow-md',
              ]">
              Academies ({{ academyCount }})
            </button>
            <button
              @click="setFilter('afterschool')"
              :class="[
                'px-6 py-2 rounded-full text-sm font-semibold transition-all duration-300 transform hover:scale-105 border-2',
                activeFilter === 'afterschool'
                  ? 'bg-[#24d3ee] text-white shadow-lg border-transparent'
                  : 'bg-gray-800/80 text-white hover:bg-[#24d3ee]/20 border-[#24d3ee]/50 hover:border-[#24d3ee] shadow-md',
              ]">
              Afterschool ({{ afterschoolCount }})
            </button>
          </div>

          <!-- Compact filter buttons when collapsed (mobile) -->
          <div v-else class="lg:hidden flex justify-center gap-2 mt-2">
            <button
              @click="setFilter('all')"
              :class="[
                'px-4 py-1 rounded-full text-xs font-bold transition-all duration-300 border-2',
                activeFilter === 'all'
                  ? 'bg-gradient-to-r from-[#FE2AF7] to-[#24d3ee] text-white border-transparent'
                  : 'bg-gray-800/80 text-white border-gray-600 shadow-md',
              ]">
              All
            </button>
            <button
              @click="setFilter('academy')"
              :class="[
                'px-4 py-1 rounded-full text-xs font-bold transition-all duration-300 border-2',
                activeFilter === 'academy' ? 'bg-[#FE2AF7] text-white border-transparent' : 'bg-gray-800/80 text-white border-[#FE2AF7]/50 shadow-md',
              ]">
              Academies
            </button>
            <button
              @click="setFilter('afterschool')"
              :class="[
                'px-4 py-1 rounded-full text-xs font-bold transition-all duration-300 border-2',
                activeFilter === 'afterschool' ? 'bg-[#24d3ee] text-white border-transparent' : 'bg-gray-800/80 text-white border-[#24d3ee]/50 shadow-md',
              ]">
              Afterschool
            </button>
          </div>
        </div>

        <!-- Location list -->
        <div :class="['p-6 overflow-y-auto scroll-smooth transition-all duration-300', panelCollapsed ? 'h-20' : 'h-80']" @wheel.stop>
          <div v-if="!panelCollapsed" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4">
            <div
              v-for="location in filteredLocations"
              :key="location.id"
              @click="focusOnLocation(location)"
              class="group bg-gray-900/60 backdrop-blur-sm rounded-2xl border border-gray-800 hover:border-[#FE2AF7]/50 hover:bg-gray-800/80 cursor-pointer transition-all duration-300 hover:shadow-2xl hover:scale-105 p-4">
              <div class="flex items-start justify-between mb-3">
                <div class="flex-1">
                  <h4 class="font-bold text-white text-base mb-1 group-hover:text-[#FE2AF7] transition-colors">
                    {{ location.name }}
                  </h4>
                  <p class="text-sm text-gray-400 mb-3">{{ location.address }}</p>

                  <div class="flex items-center flex-wrap gap-2 mb-3">
                    <span
                      :class="[
                        'px-3 py-1 rounded-full text-xs font-bold text-white shadow-lg',
                        location.type === 'academy' ? 'bg-[#FE2AF7] shadow-[#FE2AF7]/20' : 'bg-[#24d3ee] shadow-[#24d3ee]/20',
                      ]">
                      {{ location.type === 'academy' ? 'Academy' : 'Afterschool' }}
                    </span>
                    <span
                      :class="[
                        'px-3 py-1 rounded-full text-xs font-bold text-white shadow-lg',
                        location.city === 'Dubai'
                          ? 'bg-gradient-to-r from-blue-500 to-indigo-600 shadow-blue-500/20'
                          : 'bg-gradient-to-r from-orange-500 to-red-600 shadow-orange-500/20',
                      ]">
                      {{ location.city }}
                    </span>
                  </div>

                  <div v-if="location.schedule" class="text-xs text-gray-400 mb-3 font-medium">
                    {{ location.schedule }}
                  </div>
                </div>
              </div>

              <button
                @click.stop="openDirections(location)"
                class="w-full bg-gradient-to-r from-[#FE2AF7] to-[#24d3ee] text-white px-4 py-2 rounded-xl text-sm font-semibold hover:shadow-lg hover:shadow-[#FE2AF7]/30 transition-all duration-300 hover:scale-105 flex items-center justify-center gap-2">
                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M9 20l-5.447-2.724A1 1 0 013 16.382V5.618a1 1 0 011.447-.894L9 7m0 13l6-3m-6 3V7m6 10l4.553 2.276A1 1 0 0021 18.382V7.618a1 1 0 00-1.447-.894L15 4m0 13V4m0 0L9 7"></path>
                </svg>
                Get Directions
              </button>
            </div>
          </div>

          <!-- Collapsed state content -->
          <div v-else class="flex flex-col items-center justify-center h-full gap-2">
            <p class="text-gray-400 text-sm">{{ filteredLocations.length }} locations available</p>
            <div class="text-xs text-gray-500">
              Filter:
              <span class="text-[#FE2AF7] font-medium">
                {{ activeFilter === 'all' ? 'All Locations' : activeFilter === 'academy' ? 'Academies' : 'Afterschool' }}
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* Custom scrollbar */
.overflow-y-auto::-webkit-scrollbar {
  width: 6px;
}

.overflow-y-auto::-webkit-scrollbar-track {
  background: rgba(31, 41, 55, 0.5);
  border-radius: 3px;
}

.overflow-y-auto::-webkit-scrollbar-thumb {
  background: linear-gradient(to bottom, #fe2af7, #24d3ee);
  border-radius: 3px;
}

.overflow-y-auto::-webkit-scrollbar-thumb:hover {
  background: linear-gradient(to bottom, #e018d4, #1fb8d4);
}

/* Remove default leaflet styling */
:global(.custom-div-icon) {
  background: transparent !important;
  border: none !important;
}

:global(.user-location-icon) {
  background: transparent !important;
  border: none !important;
}

/* Animation for pulse effect */
@keyframes pulse {
  0%,
  100% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.05);
  }
}

.animate-pulse {
  animation: pulse 2s infinite;
}
</style>
