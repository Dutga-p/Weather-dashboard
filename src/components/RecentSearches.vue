<script setup>
defineProps({
  cities: Array,
  darkMode: Boolean,
});

defineEmits(["select-city"]);
</script>

<template>
  <div v-if="cities.length > 0" class="max-w-4xl mx-auto px-4 mt-8">
    <h3
      class="text-xl font-semibold mb-4"
      :class="darkMode ? 'text-white' : 'text-gray-800'"
    >
      Búsquedas Recientes
    </h3>
    <div class="flex flex-wrap gap-3">
      <button
        v-for="city in cities"
        :key="city"
        @click="$emit('select-city', city)"
        class="px-5 py-2 rounded-full font-medium transition-all duration-300 hover:scale-105 shadow-md"
        :class="
          darkMode
            ? 'bg-gray-700 text-white hover:bg-gray-600'
            : 'bg-white text-gray-800 hover:bg-gray-100'
        "
      >
        {{ city }}
      </button>
    </div>
  </div>
</template>
