<script setup>
import { ref } from "vue";

defineProps({
  darkMode: Boolean,
});

const emit = defineEmits(["search"]);
const searchQuery = ref("");

const handleSearch = () => {
  if (searchQuery.value.trim()) {
    emit("search", searchQuery.value);
    searchQuery.value = "";
  }
};
</script>

<template>
  <div class="max-w-2xl mx-auto mb-8 px-4">
    <div class="relative">
      <input
        v-model="searchQuery"
        @keyup.enter="handleSearch"
        type="text"
        placeholder="Buscar ciudad..."
        class="w-full px-6 py-4 rounded-2xl shadow-lg focus:outline-none focus:ring-4 transition-all duration-300"
        :class="
          darkMode
            ? 'bg-gray-800 text-white focus:ring-blue-500/50 placeholder-gray-400'
            : 'bg-white text-gray-900 focus:ring-blue-300 placeholder-gray-500'
        "
      />
      <button
        @click="handleSearch"
        class="absolute right-3 top-1/2 transform -translate-y-1/2 p-2 rounded-xl transition-all duration-300 hover:scale-110"
        :class="
          darkMode
            ? 'bg-blue-600 hover:bg-blue-700'
            : 'bg-blue-500 hover:bg-blue-600'
        "
      >
        <svg
          class="w-6 h-6 text-white"
          fill="none"
          stroke="currentColor"
          viewBox="0 0 24 24"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"
          />
        </svg>
      </button>
    </div>
  </div>
</template>
