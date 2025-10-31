<script setup>
defineProps({
  weather: Object,
  darkMode: Boolean,
});

const getWeatherIcon = (icon) => {
  return `https://openweathermap.org/img/wn/${icon}@4x.png`;
};

const formatDate = (timestamp) => {
  return new Date(timestamp * 1000).toLocaleDateString("es-ES", {
    weekday: "long",
    year: "numeric",
    month: "long",
    day: "numeric",
  });
};
</script>

<template>
  <Transition name="fade">
    <div v-if="weather" class="max-w-4xl mx-auto px-4">
      <div
        class="weather-card rounded-3xl shadow-2xl p-8"
        :class="
          darkMode
            ? 'bg-gradient-to-br from-gray-800 to-gray-900'
            : 'bg-gradient-to-br from-blue-400 to-purple-500'
        "
      >
        <div class="text-center mb-6">
          <h2 class="text-4xl font-bold text-white mb-2">
            {{ weather.name }}, {{ weather.sys.country }}
          </h2>
          <p class="text-white/80 text-lg">{{ formatDate(weather.dt) }}</p>
        </div>

        <div class="flex flex-col md:flex-row items-center justify-around mb-8">
          <div class="text-center mb-6 md:mb-0">
            <img
              :src="getWeatherIcon(weather.weather[0].icon)"
              :alt="weather.weather[0].description"
              class="w-40 h-40 mx-auto drop-shadow-2xl"
            />
            <p class="text-white text-2xl font-semibold capitalize">
              {{ weather.weather[0].description }}
            </p>
          </div>

          <div class="text-center">
            <div class="text-8xl font-bold text-white mb-4">
              {{ Math.round(weather.main.temp) }}°C
            </div>
            <div class="flex gap-6 justify-center text-white/90">
              <div>
                <p class="text-sm">Máx</p>
                <p class="text-xl font-semibold">
                  {{ Math.round(weather.main.temp_max) }}°
                </p>
              </div>
              <div>
                <p class="text-sm">Mín</p>
                <p class="text-xl font-semibold">
                  {{ Math.round(weather.main.temp_min) }}°
                </p>
              </div>
            </div>
          </div>
        </div>

        <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
          <div class="bg-white/10 backdrop-blur-sm rounded-2xl p-4 text-center">
            <svg
              class="w-8 h-8 mx-auto mb-2 text-white"
              fill="currentColor"
              viewBox="0 0 20 20"
            >
              <path
                d="M10 2a1 1 0 011 1v1.323l3.954 1.582 1.599-.8a1 1 0 01.894 1.79l-1.233.616 1.738 5.42a1 1 0 01-.285 1.05A3.989 3.989 0 0115 15a3.989 3.989 0 01-2.667-1.019 1 1 0 01-.285-1.05l1.738-5.42-1.233-.617a1 1 0 01.894-1.788l1.599.799L11 4.323V3a1 1 0 011-1zm-5 8.274l-.818 2.552c-.25.78.08 1.632.832 1.952.173.073.357.122.543.122.296 0 .591-.067.865-.196a1.978 1.978 0 00.832-1.952L7.82 10.274 5 8.274z"
              />
            </svg>
            <p class="text-white/70 text-sm">Sensación</p>
            <p class="text-white text-xl font-semibold">
              {{ Math.round(weather.main.feels_like) }}°C
            </p>
          </div>

          <div class="bg-white/10 backdrop-blur-sm rounded-2xl p-4 text-center">
            <svg
              class="w-8 h-8 mx-auto mb-2 text-white"
              fill="currentColor"
              viewBox="0 0 20 20"
            >
              <path
                fill-rule="evenodd"
                d="M5.5 2a3.5 3.5 0 101.665 6.58L8.585 10l-1.42 1.42a3.5 3.5 0 101.414 1.414l8.128-8.127a1 1 0 00-1.414-1.414L10 8.586 8.58 7.165A3.5 3.5 0 005.5 2zM4 5.5a1.5 1.5 0 113 0 1.5 1.5 0 01-3 0z"
                clip-rule="evenodd"
              />
            </svg>
            <p class="text-white/70 text-sm">Humedad</p>
            <p class="text-white text-xl font-semibold">
              {{ weather.main.humidity }}%
            </p>
          </div>

          <div class="bg-white/10 backdrop-blur-sm rounded-2xl p-4 text-center">
            <svg
              class="w-8 h-8 mx-auto mb-2 text-white"
              fill="currentColor"
              viewBox="0 0 20 20"
            >
              <path
                fill-rule="evenodd"
                d="M10 18a8 8 0 100-16 8 8 0 000 16zM4.332 8.027a6.012 6.012 0 011.912-2.706C6.512 5.73 6.974 6 7.5 6A1.5 1.5 0 019 7.5V8a2 2 0 004 0 2 2 0 011.523-1.943A5.977 5.977 0 0116 10c0 .34-.028.675-.083 1H15a2 2 0 00-2 2v2.197A5.973 5.973 0 0110 16v-2a2 2 0 00-2-2 2 2 0 01-2-2 2 2 0 00-1.668-1.973z"
                clip-rule="evenodd"
              />
            </svg>
            <p class="text-white/70 text-sm">Presión</p>
            <p class="text-white text-xl font-semibold">
              {{ weather.main.pressure }} hPa
            </p>
          </div>

          <div class="bg-white/10 backdrop-blur-sm rounded-2xl p-4 text-center">
            <svg
              class="w-8 h-8 mx-auto mb-2 text-white"
              fill="currentColor"
              viewBox="0 0 20 20"
            >
              <path
                fill-rule="evenodd"
                d="M3 3a1 1 0 000 2v8a2 2 0 002 2h2.586l-1.293 1.293a1 1 0 101.414 1.414L10 15.414l2.293 2.293a1 1 0 001.414-1.414L12.414 15H15a2 2 0 002-2V5a1 1 0 100-2H3zm11 4a1 1 0 10-2 0v4a1 1 0 102 0V7zm-3 1a1 1 0 10-2 0v3a1 1 0 102 0V8zM8 9a1 1 0 00-2 0v2a1 1 0 102 0V9z"
                clip-rule="evenodd"
              />
            </svg>
            <p class="text-white/70 text-sm">Viento</p>
            <p class="text-white text-xl font-semibold">
              {{ Math.round(weather.wind.speed * 3.6) }} km/h
            </p>
          </div>
        </div>
      </div>
    </div>
  </Transition>
</template>
