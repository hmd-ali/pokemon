<template>
  <div class="about">
    <div v-if="pokemon" class="flex text-white justify-center flex-col items-center max-w-2xl m-auto">
      <h3 class="text-2xl capitalize text-green-500">{{pokemon.name}}</h3>
      <div class="flex justify-center items-center">
        <img v-if="pokemon.sprites.front_shiny" class="w-40" :src="pokemon.sprites.front_shiny" alt="">
        <img v-if="pokemon.sprites.back_shiny" class="w-40" :src="pokemon.sprites.back_shiny" alt="">
      </div>
      <div class="flex justify-center flex-wrap p-4">
        <h3 class="m-1">Height: {{pokemon.height}}</h3>
        <h3 class="m-1">Weight: {{pokemon.weight}}</h3>
      </div>
      <div class="flex flex-col max-w-md m-auto items-center justify-center">
        <h3 class="text-xl ">Stats</h3>
        <div class="flex p-4 flex-wrap max-w-md m-auto items-center justify-center">
          <div v-for="(stat,idx) in pokemon.stats" :key="idx" class="rounded border-black border-2 m-1.5">
            <h3 class="capitalize">Base {{stat.stat.name}}: {{stat.base_stat}}</h3>
          </div>
        </div>
      </div>
      <div class="flex flex-col items-center justify-center">
        <h1 class="text-xl">Types</h1>
        <div class="flex flex-col items-center justify-center">
          <h3 class="capitalize m-1" v-for="(type,idx) in pokemon.types" :key="idx">{{type.type.name}}</h3>
        </div>
      </div>
    </div>
    <div v-else class="flex justify-center align-center">
      <h1 class="m-auto text-2xl text-white">No data available for this pokemon</h1>
    </div>
  </div>
</template>

<script>

import { useRoute } from 'vue-router'
import { reactive, toRefs } from 'vue'

export default {
  setup(){
    const route = useRoute()
    const state = reactive({
      pokemon: null
    })

    fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.slug}`)
      .then((res) => res.json())
      .then((data) => {
        console.log(data)
        state.pokemon = data
      })

    return {...toRefs(state)}
  }
}
</script>