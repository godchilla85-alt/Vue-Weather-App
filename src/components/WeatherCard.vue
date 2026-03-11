<script setup>
import BorderLine from "./BorderLine.vue";
import Forecast from "./Forecast.vue";
import { Wind, Droplets, CloudRain } from 'lucide-vue-next';


defineProps({
  place: Object,
});

</script>

<template>
  <div class="flex flex-col text-white lg:p-10 py-10 px-5 gap-6 mb-6 relative overflow-hidden">
    <div class="flex justify-center">
      <p class="lg:text-[2em]">{{ place.location.name }}, {{ place.location.country }}</p>
    </div>
    <div class="flex justify-center">
      <h1 class="text-9xl m-5">{{ Math.round(place.current.temp_c) }}&deg;</h1>
    </div>
    <div class="flex w-full p-3">
      <div
        class="flex items-center gap-3 px-6 py-2 bg-white/20 backdrop-blur-md border border-white/30 rounded-full text-white shadow-lg w-fit mx-auto"
      >
        <img
          :src="place.current.condition.icon"
          alt="icon"
          class="w-12 h-12 -my-2"
        />

        <p class="text-lg font-medium">
          {{ place.current.condition.text }}
        </p>
      </div>

      
    </div>

    <div class="flex w-full justify-around lg:px-10 mt-10">
        <div class="flex flex-col w-1/3 items-center">
          <CloudRain class="w-8 h-8 text-white" />
          <p>{{ place.forecast.forecastday[0].day.daily_chance_of_rain }}%</p>
          <p class="text-sm">Chance of rain</p>
        </div>
        <div class="flex flex-col w-1/3 items-center">
          <Wind class="w-8 h-8 text-white" />
          <p>{{ place.current.wind_kph }} km/h</p>
          <p class="text-sm">Windspeed</p>
        </div>
        <div class="flex flex-col w-1/3 items-center">
          <Droplets class="w-8 h-8 text-white" />
          <p>{{ place.current.humidity }}%</p>
          <p class="text-sm">Humidity</p>
        </div>
        
      </div>
    <BorderLine class="mt-10" />

    <div class="rounded-3xl">
      <div class="flex gap-4 pb-4 overflow-auto">
        <div
          v-for="(day, idx) in place.forecast.forecastday"
          :key="idx"
          class="flex-1"
        >
          <Forecast :day="day" />
        </div>
      </div>
    </div>
  </div>
</template>
