<template>
  <div class="w-full flex justify-center">
    <input 
      type="text" 
      placeholder="Enter Pokemon Name"
      class="mt-1 p-2 border-2 border-white rounded-lg hover:border-yellow-300 outline-none focus:border-yellow-500 transition-all placeholder-yellow-300"
      v-model="text"
     >
  </div>
  <div class="mt-10 p-4 flex flex-wrap items-center justify-center">
    <div
      class="mt-4 p-4 text-2xl text-blue-500"
      v-for="(pokemon, idx) in filteredPokemons"
      :key="idx"
    >
      <router-link :to="`/about/${urlIdLookup[pokemon.name]}`">
        {{ pokemon.name }}
      </router-link>
    </div>
  </div>
</template>

<script>

import { reactive, toRefs, computed } from 'vue'

export default {
  name: 'Home',
  setup(){

    const state = reactive({
      pokemons: [],
      urlIdLookup: {},
      text: "",
      filteredPokemons: computed(() => updatePokemons())
    })

    const updatePokemons = () => {
      if(!state.text || state.text.length < 3){
        return
      }
      console.log(state.filteredPokemons)
      return state.pokemons.filter((pokemon) =>
        pokemon.name.includes(state.text)
      )
    }

    fetch('https://pokeapi.co/api/v2/pokemon?limit=1000&offset=0')
      .then((res) => res.json())
      .then((data) => {
        console.log(data)
        state.pokemons = data.results
        state.urlIdLookup = data.results.reduce((acc, curr, idx) => 
          acc = {...acc,[curr.name]: idx+1}
        ,{})
      })
    
    return { ...toRefs(state)}
  }
}
</script>
