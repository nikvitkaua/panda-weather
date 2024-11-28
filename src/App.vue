<script setup>
import { ref, onMounted } from 'vue';
import TheHeader from '@/components/TheHeader.vue'
import AppSearch from '@/components/AppSearch.vue'
import AppBody from '@/components/AppBody.vue'
import { API_WEATHER_KEY } from '@/assets/constants/constants.js'

const weatherData = ref(null);
const isLoading = ref(true);
const error = ref('');

async function fetchWeatherData(city = 'Kyiv') {
  isLoading.value = true;
  error.value = '';

  try {
    const response = await fetch(
      `https://api.openweathermap.org/data/2.5/forecast?q=${city}&appid=${API_WEATHER_KEY}&units=metric`
    );
    if (!response.ok) throw new Error('Failed to fetch weather data');
    const data = await response.json();
    weatherData.value = data;
  } catch (err) {
    error.value = err.message;
  } finally {
    isLoading.value = false;
  }
}

onMounted(fetchWeatherData);
</script>

<template>
  <div class="container">
    <TheHeader />
    <main>
      <AppSearch :fetchWeatherData="fetchWeatherData" />
      <AppBody :weatherData="weatherData" :isLoading="isLoading" :error="error" />
    </main>
  </div>
</template>
