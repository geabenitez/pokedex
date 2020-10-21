<template>
  <div class="flex flex-col h-screen p-5 font-body space-y-5">
    <div>
      <span class="font-bold text-xl">Pokedex</span>
    </div>
    <div class="grid grid-cols-2 gap-3">
      <a v-for="(elemento, index) of pokemons" :key="index" class="h-32 col-span-1 rounded-2xl bg-gray-400" target="_blank" :href="elemento.url">
        <span>{{ elemento.name }}</span>
      </a>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  created(){
    axios.get('https://pokeapi.co/api/v2/pokemon').then((params) => {
      console.log(params.data.results)
      this.pokemons = params.data.results
    })
  },
  data() {
    return {
      pokemons: []
    }
  }
}
</script>
