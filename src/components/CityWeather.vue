<script setup>
import WeatherChart from '@/components/elements/WeatherChart.vue'
import { ref } from 'vue';

defineProps({
  weatherData: Object,
  isLoading: Boolean,
  error: String,
});


const isFavorite = ref(false);

function toggleFavorite() {
  isFavorite.value = !isFavorite.value;
}

function removeCity() {
  console.log('Город удалён');
}
</script>

<template>
  <div class="city">
    <div class="city__operations">
      <div
        class="city__toFavorite"
        :class="{ 'active': isFavorite }"
        @click="toggleFavorite"
      >
        <span v-if="!isFavorite">☆</span>
        <span v-else>★</span>
      </div>

      <div
        class="city__remove"
        @click="removeCity"
      >
        &#x2716;
      </div>
    </div>

    <div class="city__inner">
      <p v-if="isLoading">Loading...</p>
      <p v-else-if="error">{{ error }}</p>
      <p v-else-if="!weatherData">No data available</p>
      <div v-else>
        <h2 class="city__name">{{ weatherData.city.name }}</h2>
        <p class="city__temperature">
          Current Temperature:
          <span>{{ Math.round(weatherData.list[0].main.temp) }}°C</span>
        </p>
        <p class="city__condition">
          Weather:
          <span>{{ weatherData.list[0].weather[0].description }}</span>
        </p>
        <p class="city__humidity">
          Humidity:
          <span>{{ weatherData.list[0].main.humidity }}%</span>
        </p>
        <p class="city__wind">
          Wind Speed:
          <span>{{ weatherData.list[0].wind.speed }} m/s</span>
        </p>
      </div>
    </div>

    <div class="city__graphic" v-if="weatherData">
      <WeatherChart
        :labels="weatherData.list.slice(0, 8).map(entry => entry.dt_txt.split(' ')[1])"
        :data="weatherData.list.slice(0, 8).map(entry => entry.main.temp)"
      />
    </div>
  </div>
</template>


<style scoped>
.city {
  position: relative;
  padding: 30px;
  border-radius: var(--border-radius);
  background-color: rgba(236, 236, 236, .7);
}

.city__btns {
  display: flex;
  align-items: center;
  gap: 20px;
  margin-bottom: 20px;
}

.city__inner {
  margin-top: 20px;
  padding: 20px;
  background-color: rgba(255, 255, 255, 0.9);
  border-radius: var(--border-radius);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.city__name {
  font-size: 24px;
  font-weight: bold;
  color: #333;
  margin-bottom: 10px;
}

.city__temperature {
  font-size: 18px;
  color: #ff7043;
}

.city__condition,
.city__humidity,
.city__wind {
  font-size: 16px;
  color: #555;
}

.city__condition span,
.city__humidity span,
.city__wind span {
  font-weight: bold;
  color: #333;
}

.city__operations {
  display: flex;
  justify-content: end;
  align-items: center;
  gap: 20px;
}

.city__toFavorite, .city__remove {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  cursor: pointer;
  transition: background-color 0.3s, transform 0.2s;
}

.city__toFavorite {
  background: linear-gradient(135deg, #f1c40f, #f39c12);
}

.city__toFavorite.active {
  background: linear-gradient(135deg, #2ecc71, #27ae60);
}

.city__toFavorite:hover {
  transform: scale(1.1);
}

.city__remove {
  background: #e74c3c;
}

.city__remove:hover {
  transform: scale(1.1);
}

.city__remove:active {
  background: #c0392b;
}

.city__toFavorite span, .city__remove span {
  font-size: 20px;
  color: white;
}
</style>
