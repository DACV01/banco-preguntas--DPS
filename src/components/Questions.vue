<template>
  <div class="px-4 pb-5">
    <div class="max-w-4xl mx-auto">
      <!-- Título principal -->
      <h1
        class="text-4xl font-extrabold text-center mb-10 text-black bg-gray-50/50 backdrop-blur-sm p-3 rounded-xl drop-shadow-sm inline-block">
        Preguntas Frecuentes
      </h1>
      <!-- Sección principal -->
      <div class="bg-gray-50/50 backdrop-blur-sm border border-gray-200/60 rounded-3xl shadow-2xl overflow-hidden">
        <!-- Categorías -->
        <div class="p-6 bg-gray-50/50 border-b border-gray-200">

          <div class="flex flex-wrap gap-3 justify-center sm:justify-start">
            <button v-for="category in categories" :key="category" @click="selectCategory(category)"
              class="px-5 py-2 rounded-full text-sm font-semibold transition duration-300 ease-in-out transform hover:scale-105 focus:outline-none"
              :class="[
                selectedCategory === category
                  ? 'bg-indigo-600 text-white shadow-md'
                  : 'bg-gray-200 text-gray-700 hover:bg-gray-300 hover:text-gray-900',
              ]">
              {{ category }}
            </button>
            <button @click="selectCategory('')"
              class="px-5 py-2 rounded-full text-sm font-semibold transition duration-300 ease-in-out transform hover:scale-105 focus:outline-none"
              :class="[
                selectedCategory === ''
                  ? 'bg-indigo-600 text-white shadow-md'
                  : 'bg-gray-200 text-gray-700 hover:bg-gray-300 hover:text-gray-900',
              ]">
              TODAS
            </button>
          </div>
        </div>

        <!-- Subcategorías -->
        <div v-if="subcategories.length > 0" class="p-4 bg-gray-100/50">
          <div class="flex flex-wrap gap-3 justify-center sm:justify-start">
            <button v-for="subcategory in subcategories" :key="subcategory" @click="selectSubcategory(subcategory)"
              class="px-4 py-2 rounded-full text-sm font-medium transition duration-300 ease-in-out transform hover:scale-105 focus:outline-none"
              :class="[
                selectedSubcategory === subcategory
                  ? 'bg-purple-500 text-white shadow-md'
                  : 'bg-gray-200 text-gray-700 hover:bg-gray-300 hover:text-gray-900',
              ]">
              {{ subcategory }}
            </button>
          </div>
        </div>

        <!-- Preguntas Frecuentes -->
        <div class="divide-y divide-gray-200">
          <div v-for="(faq, index) in filteredFAQs" :key="index"
            class="transition-colors duration-200 hover:bg-gray-50/80">
            <button @click="toggleFAQ(index)" class="w-full px-6 py-5 text-left focus:outline-none group"
              :class="[openFAQs[index] ? 'bg-gray-50/80' : '']">
              <div class="flex items-center justify-between">
                <span class="text-lg font-semibold text-gray-900 group-hover:text-indigo-600 transition"
                  :class="[openFAQs[index] ? 'text-indigo-600' : '']">
                  {{ faq.question }}
                </span>
                <ChevronDownIcon :class="[
                  'w-6 h-6 text-gray-500 transition-all duration-300 group-hover:text-indigo-600',
                  openFAQs[index] ? 'rotate-180 text-indigo-600' : '',
                ]" />
              </div>
            </button>
            <div v-show="openFAQs[index]" class="px-6 py-4 bg-gray-50/80 text-gray-700 rounded-b-lg">
              <p class="mb-3 text-gray-800">{{ faq.answer }}</p>
              <p class="text-sm text-gray-500">
                Categoría:
                <span class="font-semibold text-indigo-600">{{
                  faq.category
                  }}</span>
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup>
import { ref, computed } from 'vue';
import { ChevronDownIcon } from 'lucide-vue-next';

  const faqData = [
    {
      question: '¿Puedo Cobrar en otro Departamento?',
      answer: 'El beneficiario debe solicitar el traslado en la regional inscrito',
      category: 'DEVOLUCION IVA',
    },
    {
      question: '¿Otra persona puede realizar el retiro?',
      answer: 'No, Unicamente el beneficiario.',
      category: 'RENTA CIUDADANA',
    },
    {
      question: '¿Que programas hay para la población Víctima del Desplazamiento Forzado?',
      answer: 'Prosperidad Social a  través de la Dirección de Inclusión Productiva tiene el programa Familias en Su Tierra, un programa de acompañamiento social para las víctimas residen el el área rural tiene 4 componentes, seguridad alimentaria, mejoramiento de condiciones de habitabilidad, proyecto productivo y fortalecimiento social y comunitario, también esta Mi Negocio apoyo a pequeños emprendimientos, Emprendimiento Colectivo par asociaciones con un 70% de población víctima.',
      category: 'INCLUSION PRODUCTIVA',
      subcategory: 'Familias en su Tierra',
    },
    {
      question: '¿En caso de no cobro, el pago se acumula?',
      answer: 'Si, el mes siguiente el pago se acumula.',
      category: 'RENTA CIUDADANA',
    },
    {
      question: 'Como Ingreso Al Programa Renta Joven ? ',
      answer: '1.Tener entro 14 y 28 años de edad.  2.Contar con un registro de graduado como bachiller(11)°. 3.Estar registrado en el SISBEN vigente  o en el instrumento de focalizacion. 4.Estar registrado en los listados censales de población indígena administrados por el Ministerio del Interior',
      category: 'RENTA JOVEN',
    },
    {
    question: '	Cuanto Se Recibe En El Programa Renta Joven ?',
      answer: 'Si, el mes siguiente el pago se acumula.',
      category: 'RENTA JOVEN',
    },
    {
    question: ' Que Instituciones De Educación Complementaria Aplican Para El Programa Renta Joven? ?',
      answer: 'Si, el mes siguiente el pago se acumula.',
      category: 'RENTA JOVEN',
    },
    {
    question: ' Puedo Cambiarme De Programa Academico?   ',
      answer: 'Si, el mes siguiente el pago se acumula.',
      category: 'RENTA JOVEN',
    },
    {
      question: '¿Cómo accedo a los programas para víctimas?',
      answer: 'Para acceder debe estar focalizado el municipio, residir en la zona Rural para Familias en Su Tierra o en la parte Urbana para Manos que Alienta, estar dentro del registro Único de victimas con el hecho victimizante desplazamiento forzado',
      category: 'INCLUSION PRODUCTIVA',
      subcategory: 'Mi Negocio',
    },
  ];

  const searchTerm = ref('');
  const selectedCategory = ref('');
  const selectedSubcategory = ref('');
  const openFAQs = ref({});

  const categories = computed(() => [
    ...new Set(faqData.map((faq) => faq.category)),
  ]);
  const subcategories = computed(() => {
    if (!selectedCategory.value) return [];
    return [
      ...new Set(
        faqData
          .filter(
            (faq) => faq.category === selectedCategory.value && faq.subcategory,
          )
          .map((faq) => faq.subcategory),
      ),
    ];
  });
  const filteredFAQs = computed(() => {
  const searchWords = searchTerm.value
    .toLowerCase()
    .split(" ")
    .filter((word) => word.trim() !== ""); // Separa en palabras y elimina vacíos

  return faqData.filter((faq) => {
    const matchesSearchTerm = searchWords.every((word) =>
      faq.question.toLowerCase().includes(word)
    ); // Verifica que todas las palabras estén en la pregunta

    const matchesCategory =
      selectedCategory.value === "" || faq.category === selectedCategory.value;

    const matchesSubcategory =
      !selectedSubcategory.value ||
      faq.subcategory === selectedSubcategory.value;

    return matchesSearchTerm && matchesCategory && matchesSubcategory;
  });
});

  const selectCategory = (category) => {
    selectedCategory.value = category;
    selectedSubcategory.value = '';
    openFAQs.value = {};
  };

  const selectSubcategory = (subcategory) => {
    selectedSubcategory.value = subcategory;
  };


  const toggleFAQ = (index) => {
    openFAQs.value = {};
    openFAQs.value[index] = !openFAQs.value[index];
  };
  const searchFAQs = () => {
    searchTerm.value = document.getElementById('search-dropdown').value.trim(); // Actualiza el término de búsqueda de forma reactiva
  };
</script>
