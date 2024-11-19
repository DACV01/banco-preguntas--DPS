<template>
  <div class="min-h-screen bg-gradient-to-br from-gray-100 to-gray-200 py-12 px-4 sm:px-6 lg:px-8">
    <div class="max-w-3xl mx-auto">
      <h1 class="div-title">
        Preguntas Frecuentes
      </h1>

      <div class="flex flex-wrap gap-2 justify-center sm:justify-start">
        <div class="flex gap-4 mb-4"></div>
        <div class="flex flex-wrap gap-2">
          <button v-for="category in categories" :key="category" @click="selectCategory(category)" :class="[
            'px-3 py-1 rounded-full text-sm font-medium transition-all duration-200',
            selectedCategory === category
              ? 'bg-white text-gray-900 border-2 border-indigo-600'
              : 'bg-gray-200 text-gray-700 hover:bg-gray-300'
          ]">
            {{ category }}
          </button>
          <button @click="selectCategory('')" :class="[
            'px-3 py-1 rounded-full text-sm font-medium transition-all duration-200',
            selectedCategory === ''
              ? 'bg-indigo-600 text-white'
              : 'bg-gray-200 text-gray-700 hover:bg-gray-300'
          ]">
            TODAS
          </button>
          

          <div v-if="subcategories.length > 0" class="flex flex-wrap gap-2 mt-4">
            <!-- Título de Subcategoría -->
            
            <!-- Botones de Subcategorías -->
            <button v-for="subcategory in subcategories" :key="subcategory" @click="selectSubcategory(subcategory)"
              :class="[
                'sub-cat',
                selectedSubcategory === subcategory ? 'bg-white text-gray-900 border-2 border-indigo-600' : 'bg-gray-200 text-gray-700 hover:bg-gray-300'
              ]"><div class="w-full">
                  <h2 class="text-lg font-semibold text-gray-800 mb-2">PROGRAMA</h2>
                </div>
              {{ subcategory }}
            </button>
          </div>

        </div>
      </div>

      <div class="faq-container">
        <div v-for="(faq, index) in filteredFAQs" :key="index" class="border-b border-gray-200 last:border-b-0">
          <button @click="toggleFAQ(index)" class="w-full px-6 py-4 text-left focus:outline-none focus:bg-gray-50">
            <div class="flex items-center justify-between">
              <span class="text-lg font-medium text-gray-900">{{ faq.question }}</span>
              <ChevronDownIcon :class="[
                'w-5 h-5 text-gray-500',
                openFAQs[index] ? 'transform rotate-180' : ''
              ]" />
            </div>
          </button>
          <div v-show="openFAQs[index]" class="stylish-box">
            <p class="test">{{ faq.answer }}</p>
            <p class="mt-2 text-sm text-gray-500">Categoría: <b>{{ faq.category }}</b></p>
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
  {
    question: "¿Puedo Cobrar en otro Departamento?",
    answer: "Debe solicitar el traslado en la Regional que se encuentre inscrita inicialmente.  ",
    category: "DEVOLUCION IVA"
  },
  {
    question: "¿Otra persona puede realizar el retiro?",
    answer: "Unicamente el beneficiario puede realizar el retiro",
    category: "RENTA CIUDADANA"
  },
  {
    question: "¿Que programas hay para la población Víctima del Desplazamiento Forzado?",
    answer: "Con un poder debidamente autenticado debe acercarse a la Regional y realizar la solicitud.",
    category: "INCLUSION PRODUCTIVA",
    subcategory: "Familias en su Tierra"
  },
  {
    question: "¿En caso de no cobro, el pago se acumula?",
    answer: "Si, en la siguiente jornada de pagos llegara acumulado, un maximo de 3 pagos",
    category: "RENTA CIUDADANA"
  },
  {
    question: "¿Porque en la plataforma mi estado es: SUSPENDIDO?",
    answer: "Esto sucede normalmente cuando los estudiantes no han actualizado los datos en plataforma o ya terminaron el ciclo Tecnologico",
    category: "RENTA JOVEN"
  },
  {
    question: "¿Cómo accedo a los programas para víctimas?",
    answer: "Con un poder debidamente autenticado debe acercarse a la Regional y realizar la solicitud.",
    category: "INCLUSION PRODUCTIVA",
    subcategory: "Mi Negocio"
  }
]
const searchTerm = ref('')
const selectedCategory = ref('')
const openFAQs = ref({})
const viewQues = ref('')
const selectedSubcategory = ref('');

const categories = computed(() => {
  return [...new Set(faqData.map(faq => faq.category))]
})
const filteredFAQs = computed(() => {
  return faqData.filter(faq => {
    const matchesSearchTerm = faq.question.toLowerCase().includes(searchTerm.value.toLowerCase());
    const matchesCategory = selectedCategory.value === '' || faq.category === selectedCategory.value;
    const matchesSubcategory =
      selectedCategory.value !== 'INCLUSION PRODUCTIVA' ||
      (selectedSubcategory.value && faq.subcategory === selectedSubcategory.value);

    return matchesSearchTerm && matchesCategory && matchesSubcategory;
  });
});

const search = () => {
  // La búsqueda se realiza automáticamente gracias al computed property
}

const selectCategory = (category) => {
  selectedCategory.value = category
  selectedSubcategory.value = '';
  Object.keys(openFAQs.value).forEach((i) => {
    openFAQs.value[i] = false;
  });
}
const selectSubcategory = (subcategory) => {
  selectedSubcategory.value = subcategory;
};
const subcategories = computed(() => {
  if (!selectedCategory.value) return [];
  return [...new Set(faqData
    .filter(faq => faq.category === selectedCategory.value && faq.subcategory)
    .map(faq => faq.subcategory))];
})

const toggleFAQ = (index) => {
  // Si ya está abierta, simplemente ciérrala
  if (openFAQs.value[index]) {
    openFAQs.value[index] = false;
  } else {
    // Si está cerrada, cierra todas las demás y ábrela
    Object.keys(openFAQs.value).forEach((i) => {
      openFAQs.value[i] = false;
    });
    openFAQs.value[index] = true;
  }
}

</script>
