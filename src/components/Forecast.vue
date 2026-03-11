<script setup>
import BorderLine from './BorderLine.vue';
defineProps({
    day: Object
})

</script>
<template>
  <div class="flex flex-col items-center justify-center 
              bg-white/20 backdrop-blur-md 
              rounded-2xl p-3 
              text-white">
    
    <div class="h-8 w-10 my-2">
        <img :src="day.day.condition.icon" alt="icon" width="50" class="mx-auto" /></div> 
    <p class="text-lg font-semibold">{{ Math.round(day.day.maxtemp_c) }}&deg; </p>
    <p class="text-xs">{{ new Date(day.date).toLocaleDateString('en-EN', {
            weekday: "long"}) }}</p>
  </div>
</template>