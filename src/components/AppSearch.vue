<script setup>
import { ref, defineEmits } from 'vue';
import AppButton from '@/components/elements/AppButton.vue';
import { API_WEATHER_KEY } from '@/assets/constants/constants.js'

const cityName = ref('');
const suggestions = ref([]);
const isLoading = ref(false);
const error = ref('');
const emit = defineEmits(['cityAdded']);

async function fetchSuggestions(query) {
  if (!query) {
    suggestions.value = [];
    return;
  }

  try {
    const response = await fetch(
      `https://api.openweathermap.org/geo/1.0/direct?q=${query}&limit=5&appid=${API_WEATHER_KEY}`
    );

    if (!response.ok) {
      throw new Error('Failed to fetch suggestions');
    }

    const data = await response.json();
    suggestions.value = data.map((item) => item.name);
  } catch (err) {
    console.error('Error fetching suggestions:', err.message);
  }
}

function handleInputChange() {
  fetchSuggestions(cityName.value);
}

function selectSuggestion(suggestion) {
  cityName.value = suggestion;
  suggestions.value = [];
}

async function searchCity() {
  if (!cityName.value) return;

  isLoading.value = true;
  error.value = '';

  try {
    const response = await fetch(
      `https://api.openweathermap.org/data/2.5/weather?q=${cityName.value}&appid=${API_WEATHER_KEY}&units=metric`
    );

    if (!response.ok) {
      throw new Error('City not found');
    }

    const weatherData = await response.json();
    emit('cityAdded', weatherData);
    cityName.value = '';
    suggestions.value = [];
  } catch (err) {
    error.value = err.message;
  } finally {
    isLoading.value = false;
  }
}

function handleAddClick() {
  searchCity();
}
</script>

<template>
  <div class="search">
    <div class="autocomplete">
      <input
        type="text"
        v-model="cityName"
        placeholder="Find your city..."
        @input="handleInputChange"
      />
      <ul v-if="suggestions.length" class="suggestions">
        <li v-for="suggestion in suggestions" :key="suggestion" @click="selectSuggestion(suggestion)">
          {{ suggestion }}
        </li>
      </ul>
    </div>
    <AppButton @click="handleAddClick">Add</AppButton>
    <p v-if="isLoading">Loading...</p>
    <p v-if="error">{{ error }}</p>
  </div>
</template>

<style scoped>
.search {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 40px;
  width: 100%;
  background: var(--black);
  padding: 15px;
  margin-top: 20px;
  border-radius: var(--border-radius);
}

.search input {
  height: 20px;
  width: 100%;
  border-radius: var(--border-radius);
  padding: 20px;
  background: var(--black);
  outline: none;
  border: 1px solid var(--white);
  color: var(--white);
  font-size: 16px;
  font-family: var(--font);
}

.search input:focus {
  border: 1px solid var(--green-dark);
}

.search button {
  font-size: 18px;
  padding: 10px 25px;
  cursor: pointer;
  border-radius: var(--border-radius);
  border: none;
  transition: .4s;
  background-color: var(--white);
  color: var(--black);
}

.search button:hover {
  color: var(--green-dark);
}

.autocomplete {
  position: relative;
  width: 100%;
}

.suggestions {
  position: absolute;
  top: 100%;
  left: 0;
  width: 100%;
  background: var(--black);
  border: 1px solid var(--white);
  border-radius: var(--border-radius);
  max-height: 150px;
  overflow-y: auto;
  z-index: 1000;
  list-style: none;
  padding: 0;
  margin: 0;
}

.suggestions li {
  padding: 10px;
  cursor: pointer;
  color: var(--white);
}

.suggestions li:hover {
  background: var(--green-dark);
  color: var(--white);
}
</style>
