<script setup lang="ts">
import { ref } from 'vue'

// Form state
const formData = ref({
  name: '',
  email: '',
  message: ''
})

const isSubmitting = ref(false)
const isSubmitted = ref(false)

// Form submission
const submitForm = async () => {
  if (!formData.value.name || !formData.value.email || !formData.value.message) {
    return
  }

  isSubmitting.value = true
  
  try {
    // Itt implementálhatod a form submission logikát
    // pl. API hívás, email küldés, stb.
    
    // Szimulált delay
    await new Promise(resolve => setTimeout(resolve, 1000))
    
    isSubmitted.value = true
    
    // Form reset
    formData.value = {
      name: '',
      email: '',
      message: ''
    }
    
  } catch (error) {
    console.error('Hiba a form küldéskor:', error)
  } finally {
    isSubmitting.value = false
  }
}

// Success message auto-hide
watch(isSubmitted, (newValue) => {
  if (newValue) {
    setTimeout(() => {
      isSubmitted.value = false
    }, 3000)
  }
})
</script>

<template>
  <section class="container bg-[#FF5D19] rounded-3xl p-8 md:p-12 mx-auto">
    <!-- Fejléc -->
    <div class="mb-8">
      <h2 class="text-white text-3xl md:text-4xl font-bold mb-4">
        ÍRJON NEKÜNK E-MAILT
      </h2>
      <p class="text-white/90 text-lg leading-relaxed max-w-4xl">
        Kérdése van, ajánlatot kérne, vagy csak többet szeretne megtudni a teqball asztalokról? Forduljon hozzánk bizalommal – örömmel segítünk! Várjuk levelét az alábbi e-mail címen, és igyekszünk mihamarabb válaszolni.
      </p>
    </div>

    <!-- Form -->
    <form @submit.prevent="submitForm" class="space-y-6">
      <!-- Név és Email egy sorban -->
      <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
        <!-- Név mező -->
        <div>
          <input
            v-model="formData.name"
            type="text"
            placeholder="Név*"
            required
            class="w-full px-6 py-4 bg-white rounded-xl border-0 placeholder-gray-400 text-gray-800 focus:outline-none focus:ring-2 focus:ring-white/30 transition-all"
          />
        </div>
        
        <!-- Email mező -->
        <div>
          <input
            v-model="formData.email"
            type="email"
            placeholder="E-mail cím*"
            required
            class="w-full px-6 py-4 bg-white rounded-xl border-0 placeholder-gray-400 text-gray-800 focus:outline-none focus:ring-2 focus:ring-white/30 transition-all"
          />
        </div>
      </div>

      <!-- Üzenet mező -->
      <div>
        <textarea
          v-model="formData.message"
          placeholder="Üzenet"
          rows="6"
          required
          class="w-full px-6 py-4 bg-white rounded-xl border-0 placeholder-gray-400 text-gray-800 focus:outline-none focus:ring-2 focus:ring-white/30 transition-all resize-none"
        ></textarea>
      </div>

      <!-- Alsó rész: Adatvédelem és gomb -->
      <div class="flex flex-col md:flex-row md:items-end md:justify-between gap-6 pt-4">
        <!-- Adatvédelmi nyilatkozat -->
        <div class="text-white/90 text-sm max-w-md">
          A küldés gombra kattintva elfogadja az 
          <NuxtLink to="/adatvedelmi-nyilatkozat" class="underline hover:text-white transition-colors">
            Adatvédelmi Nyilatkozatot
          </NuxtLink>
        </div>

        <!-- Küldés gomb -->
        <button
          type="submit"
          :disabled="isSubmitting || !formData.name || !formData.email || !formData.message"
          class="bg-[#242424] hover:bg-[#1a1a1a] disabled:bg-gray-600 disabled:cursor-not-allowed text-white font-semibold px-8 py-4 rounded-md transition-all duration-200 flex items-center gap-3 min-w-[140px] justify-center group"
          style="border-radius: 6px;"
        >
          <span v-if="!isSubmitting">Kosárba</span>
          <span v-else>Küldés...</span>
          
          <!-- Küldés ikon -->
          <svg 
            v-if="!isSubmitting"
            class="w-5 h-5 group-hover:translate-x-1 transition-transform" 
            fill="none" 
            stroke="currentColor" 
            viewBox="0 0 24 24"
          >
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 19l9 2-9-18-9 18 9-2zm0 0v-8" />
          </svg>
          
          <!-- Loading spinner -->
          <div v-else class="w-5 h-5 border-2 border-white/30 border-t-white rounded-full animate-spin"></div>
        </button>
      </div>
    </form>

    <!-- Success message -->
    <div
      v-if="isSubmitted"
      class="mt-6 bg-white/20 backdrop-blur border border-white/30 rounded-xl p-4 text-white text-center"
    >
      <div class="flex items-center justify-center gap-2">
        <svg class="w-5 h-5 text-green-300" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
        </svg>
        <span>Köszönjük! Üzenetét sikeresen elküldtük.</span>
      </div>
    </div>
  </section>
</template>

<style scoped>
/* Custom focus styles */
input:focus,
textarea:focus {
  box-shadow: 0 0 0 3px rgba(255, 255, 255, 0.1);
}

/* Loading animation */
@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

.animate-spin {
  animation: spin 1s linear infinite;
}
</style>