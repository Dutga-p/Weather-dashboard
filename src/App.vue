<script setup>
import { ref, computed, onMounted } from "vue";
import AppHeader from "./components/AppHeader.vue";
import SearchBar from "./components/SearchBar.vue";
import WeatherCard from "./components/WeatherCard.vue";
import RecentSearches from "./components/RecentSearches.vue";

const API_KEY = "TU_API_KEY_AQUI"; // Reemplaza con tu API key
const darkMode = ref(false);
const weatherData = ref(null);
const recentCities = ref([]);
const loading = ref(false);
const error = ref("");

const bgClass = computed(() => {
  if (darkMode.value) {
    return "bg-gray-900";
  }
  return "bg-gradient-to-br from-blue-50 to-purple-50";
});

const fetchWeather = async (city) => {
  loading.value = true;
  error.value = "";

  try {
    const response = await fetch(
      `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${API_KEY}&units=metric&lang=es`
    );

    if (!response.ok) {
      throw new Error("Ciudad no encontrada");
    }

    const data = await response.json();
    weatherData.value = data;

    // Guardar en búsquedas recientes
    if (!recentCities.value.includes(city)) {
      recentCities.value.unshift(city);
      if (recentCities.value.length > 5) {
        recentCities.value.pop();
      }
      saveToLocalStorage();
    }
  } catch (err) {
    error.value = err.message;
    weatherData.value = null;
  } finally {
    loading.value = false;
  }
};

const toggleDarkMode = () => {
  darkMode.value = !darkMode.value;
  saveToLocalStorage();
};

const saveToLocalStorage = () => {
  localStorage.setItem(
    "weatherApp",
    JSON.stringify({
      darkMode: darkMode.value,
      recentCities: recentCities.value,
    })
  );
};

const loadFromLocalStorage = () => {
  const data = localStorage.getItem("weatherApp");
  if (data) {
    const parsed = JSON.parse(data);
    darkMode.value = parsed.darkMode || false;
    recentCities.value = parsed.recentCities || [];
  }
};

onMounted(() => {
  loadFromLocalStorage();
  fetchWeather("Cali");
});
</script>

<template>
  <div :class="bgClass" class="min-h-screen transition-colors duration-300">
    <AppHeader :dark-mode="darkMode" @toggle-dark-mode="toggleDarkMode" />

    <SearchBar :dark-mode="darkMode" @search="fetchWeather" />

    <div v-if="loading" class="text-center py-12">
      <div
        class="inline-block animate-spin rounded-full h-16 w-16 border-t-4 border-b-4"
        :class="darkMode ? 'border-blue-500' : 'border-blue-600'"
      ></div>
    </div>

    <div v-if="error" class="max-w-2xl mx-auto px-4">
      <div
        class="p-6 rounded-2xl text-center"
        :class="
          darkMode ? 'bg-red-900/50 text-red-200' : 'bg-red-100 text-red-800'
        "
      >
        <svg
          class="w-12 h-12 mx-auto mb-3"
          fill="currentColor"
          viewBox="0 0 20 20"
        >
          <path
            fill-rule="evenodd"
            d="M10 18a8 8 0 100-16 8 8 0 000 16zM8.707 7.293a1 1 0 00-1.414 1.414L8.586 10l-1.293 1.293a1 1 0 101.414 1.414L10 11.414l1.293 1.293a1 1 0 001.414-1.414L11.414 10l1.293-1.293a1 1 0 00-1.414-1.414L10 8.586 8.707 7.293z"
            clip-rule="evenodd"
          />
        </svg>
        <p class="text-xl font-semibold">{{ error }}</p>
      </div>
    </div>

    <WeatherCard
      v-if="!loading && weatherData"
      :weather="weatherData"
      :dark-mode="darkMode"
    />

    <RecentSearches
      :cities="recentCities"
      :dark-mode="darkMode"
      @select-city="fetchWeather"
    />
  </div>
</template>
