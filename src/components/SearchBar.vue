<script setup>
import { reactive } from 'vue';

const emit = defineEmits(['place-data']);

const searchTerm = reactive({
    query: '',
    timeout: null,
    results: null,
});

const handleSearch = () => {
    clearTimeout(searchTerm.timeout)
    searchTerm.timeout = setTimeout(async () => {
        if(searchTerm.query !== ''){
            const res = await fetch(`http://api.weatherapi.com/v1/search.json?key=40153c0d228e4670826202733262402&q=${searchTerm.query}`)
            const data = await res.json()
            searchTerm.results = data
        } else{
            searchTerm.results = null
        }
    }, 500)
    console.log('hallo')
}

const getWeather = async (id) => {
    const res = await fetch(`http://api.weatherapi.com/v1/forecast.json?key=40153c0d228e4670826202733262402&q=id:${id}&days=7&aqi=no&alerts=no
`)
const data = await res.json()

    emit('place-data', data)
    searchTerm.query = ''
    searchTerm.results = null

    console.log(data)
}
</script>
<template>
<form @submit.prevent class="mx-auto mt-5 relative">   
    <label for="search" class="block mb-2.5 text-sm font-medium text-heading sr-only ">Search</label>
    <div class="relative">
        <div class="absolute inset-y-0 flex items-center ps-3 pointer-events-none">
            <svg class="w-4 h-4 text-body" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="none" viewBox="0 0 24 24"><path stroke="currentColor" stroke-linecap="round" stroke-width="2" d="m21 21-3.5-3.5M17 10a7 7 0 1 1-14 0 7 7 0 0 1 14 0Z"/></svg>
        </div>
        <input 
        type="text" 
        id="search" 
        class="p-3 ps-9 bg-neutral-secondary-medium text-heading text-sm rounded-4xl w-[80vw] bg-white/30 backdrop-blur-md shadow-xl placeholder:text-body" 
        placeholder="Search for a place" 
        v-model="searchTerm.query"
        @input="handleSearch"
        />
        
    </div>
    <div class="bg-white my-2 rounded-lg shadow-lg w-full absolute z-10">
        <div v-if="searchTerm.results !== null">
      <div v-for="place in searchTerm.results" :key="place.id">
        <button type="button" @click="getWeather(place.id)" class="px-3 my-2 hover:text-indigo-600 hover:font-bold w-full text-left cursor-pointer">
        {{ place.name }}, {{ place.region }}, {{ place.country }}
        </button>
      </div>
      </div>
    </div>
</form>

</template>