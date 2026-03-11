<script setup>
import { ref, computed  } from 'vue';
import SearchBar from './components/SearchBar.vue';
import WeatherCard from './components/WeatherCard.vue';

import bgImageDefault from './assets/img/default-bg.jpg';
import bgImageSunny from './assets/img/sunny-bg.jpg';
import bgImageMisty from './assets/img/misty-bg.jpg';
import bgImageCloudy from './assets/img/cloudy-bg.jpg';
import bgImageSnowy from './assets/img/snowy-bg.jpg';
import bgImageRainy from './assets/img/rainy-bg.jpg';

const selectedPlace = ref(null)

const updatePlace = (data) => {
  selectedPlace.value = data
  
}

const backgroundImage = computed(() => {
  if (!selectedPlace.value) return bgImageDefault

  const condition = selectedPlace.value.current.condition.text.toLowerCase()

  if (condition.includes('sun') || condition.includes('clear')) {
    return bgImageSunny
  }

  if (condition.includes('mist') || condition.includes('fog')) {
    return bgImageMisty
  }

  if (condition.includes('cloud')) {
    return bgImageCloudy
  }

  if (condition.includes('snow')) {
    return bgImageSnowy
  }

  if (condition.includes('rain') || condition.includes('drizzle')) {
    return bgImageRainy
  }

  return bgImageDefault
});

</script>

<template>
  <div 
  class="min-h-screen flex flex-col items-center lg:justify-center gap-6 bg-cover overflow-hidden"
  :style="{ backgroundImage: `url(${backgroundImage})` }"
>
<div class="absolute inset-0 bg-black/20"></div>
    <SearchBar @place-data="updatePlace"></SearchBar>
    <div class="h-full w-[80vw] bg-white/30 rounded-4xl backdrop-blur-md shadow-xl">
      <div v-if="selectedPlace">
      <WeatherCard :place="selectedPlace"></WeatherCard>
    </div>
    </div>

  </div>
  
</template>
