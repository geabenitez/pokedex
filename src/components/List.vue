<template>
  <div class="flex flex-col h-screen p-5 font-body space-y-5 container mx-auto">
    <span class="font-bold text-xl md:text-2xl xl:text-3xl uppercase hover:text-green-700">Pokedex</span>
    <div class="grid grid-cols-2 md:grid-cols-3 xl:grid-cols-4 gap-3">
      <!-- Card -->
      <router-link v-for="(p, i) of pokemons" :key="i" :to="`pokemons/${p.name}`" tag="div" class="flex flex-col col-span-1 rounded-2xl p-4 border-2 border-gray-600 hover:shadow-lg hover:-mt-2 cursor-pointer" :class="`bg-${p.color}-500`">
        <span>{{ p.name }}</span>
        <div class="flex flex-row">
          <div class="w-1/2 flex flex-col justify-center space-y-2">
            <span class="bg-white rounded-full px-6 py-1">Poder 1</span>
            <span class="bg-white rounded-full px-6 py-1">Poder 2</span>
          </div>
          <div class="w-1/2 flex justify-end">
            <img :src="`https://pokeres.bastionbot.org/images/pokemon/${i + 1}.png`" class="h-24 w-24">
          </div>
        </div>
      </router-link>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'List',
  created(){
    // Obtiene listado de colores
    function getPokemonColors() {
      return axios.get('https://pokeapi.co/api/v2/pokemon-color/')
    }

    // obtiene listado de pokemons
    function getPokemons() {
      return axios.get('https://pokeapi.co/api/v2/pokemon') 
    }

    Promise.all([getPokemonColors(), getPokemons()])
      .then(async (res) => {
        const colors = res[0].data.results
        let pokemons = res[1].data.results
        
        // Obtiene los detalles de colores
        const fcolor = []
        for (const c of colors) {
          const res = await axios.get(c.url)
          fcolor.push(res.data)
        }

        pokemons = pokemons.map((p) => {
          const color = fcolor.find((c) => {
            return c.pokemon_species.find((s) => {
              return s.name == p.name
            })
          }).name
          return {
            ...p,
            color
          }
        })

        for (const p of pokemons) {
          const resultado = await axios.get(p.url)
          p['types'] = resultado.data.types.map((t) => {
            return t.type.name
          })
        }

        this.pokemons = pokemons
      })
  },
  data() {
    return {
      pokemons: []
    }
  }
}
</script>
