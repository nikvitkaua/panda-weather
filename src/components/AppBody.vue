<script setup>
import CityWeather from '@/components/CityWeather.vue';

const props = defineProps({
  weatherData: Object, // Данные для текущего города
  isLoading: Boolean,
  error: String,
});
</script>

<template>
  <div class="weather">
    <p v-if="isLoading">Loading weather data...</p>
    <p v-if="error">{{ error }}</p>
    <div v-if="weatherData">
      <CityWeather :weatherData="weatherData" />
    </div>
    <p v-else>No city selected. Please search for a city!</p>
  </div>
</template>

<style scoped>
.weather {
  padding: 30px;
  margin-top: 20px;
  border-radius: var(--border-radius);
  background-color: var(--black);
}
</style>
