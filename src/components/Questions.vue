<template>
  <div class="min-h-screen bg-gradient-to-br from-gray-100 to-gray-200 py-12 px-4 sm:px-6 lg:px-8">
    <div class="max-w-3xl mx-auto">
      <h1 class="text-4xl font-extrabold text-gray-900 text-center mb-8">
        Banco de Preguntas DPS Putumayo
      </h1>

      <div class="mb-8">
        <div class="flex gap-4 mb-4">
          
        </div>
        <div class="flex flex-wrap gap-2">
          <button
            @click="selectCategory('')"
            :class="[
              'px-3 py-1 rounded-full text-sm font-medium',
              selectedCategory === '' ? 'bg-indigo-600 text-white' : 'bg-gray-200 text-gray-700 hover:bg-gray-300'
            ]"
          >
            Todas
          </button>
          <button
            v-for="category in categories"
            :key="category"
            @click="selectCategory(category)"
            :class="[
              'px-3 py-1 rounded-full text-sm font-medium',
              selectedCategory === category ? 'bg-indigo-600 text-white' : 'bg-gray-200 text-gray-700 hover:bg-gray-300'
            ]"
          >
            {{ category }}
          </button>
        </div>
      </div>

      <div class="bg-white shadow-xl rounded-lg overflow-hidden">
        <div v-for="(faq, index) in filteredFAQs" :key="index" class="border-b border-gray-200 last:border-b-0">
          <button
            @click="toggleFAQ(index)"
            class="w-full px-6 py-4 text-left focus:outline-none focus:bg-gray-50"
          >
            <div class="flex items-center justify-between">
              <span class="text-lg font-medium text-gray-900">{{ faq.question }}</span>
              <ChevronDownIcon
                :class="[
                  'w-5 h-5 text-gray-500',
                  openFAQs[index] ? 'transform rotate-180' : ''
                ]"
              />
            </div>
          </button>
          <div
            v-show="openFAQs[index]"
            class="stylish-box"
          >
            <p class="test">{{ faq.answer }}</p>
            <p class="mt-2 text-sm text-gray-500">Categoría: {{ faq.category }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import { SearchIcon, ChevronDownIcon } from 'lucide-vue-next'

const faqData = [
  { question: "¿Puedo Cobrar en otro Departamento?", answer: "Debe solicitar el traslado en la Regional que se encuentre inscrita inicialmente.  ", category: "DEVOLUCION IVA" },
  { question: "¿Otra persona puede realizar el retiro?", answer: "Unicamente el beneficiario puede realizar el retiro", category: "RENTA CIUDADANA" },
  { question: "¿En caso que la persona se encuentre mal de salud y no se pueda movilizar?", answer: "Con un poder debidamente autenticado debe acercarse a la Regional y realizar la solicitud.", category: "ADULTO MAYOR" },
  { question: "¿En caso de no cobro, el pago se acumula?", answer: "Si, en la siguiente jornada de pagos llegara acumulado, un maximo de 3 pagos", category: "RENTA CIUDADANA" },
  { question: "¿Porque en la plataforma mi estado es: SUSPENDIDO?", answer: "Esto sucede normalmente cuando los estudiantes no han actualizado los datos en plataforma o ya terminaron el ciclo Tecnologico", category: "RENTA JOVEN" },
]

const searchTerm = ref('')
const selectedCategory = ref('')
const openFAQs = ref({})

const categories = computed(() => {
  return [...new Set(faqData.map(faq => faq.category))]
})

const filteredFAQs = computed(() => {
  return faqData.filter(faq => 
    faq.question.toLowerCase().includes(searchTerm.value.toLowerCase()) &&
    (selectedCategory.value === '' || faq.category === selectedCategory.value)
  )
})

const search = () => {
  // La búsqueda se realiza automáticamente gracias al computed property
}

const selectCategory = (category) => {
  selectedCategory.value = category
}

const toggleFAQ = (index) => {
  openFAQs.value[index] = !openFAQs.value[index]
}
</script>
