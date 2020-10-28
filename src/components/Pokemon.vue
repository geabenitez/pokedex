<template>
  <div class="flex flex-col">
    <span v-if="pokemon.types">{{ pokemon.types.name }}</span>
    <button class="bg-red-500" @click="tab = 'about'">About</button>
    <button class="bg-blue-500" @click="tab = 'base'">Base</button>

    <div v-if="tab == 'about'">
      about
    </div>
    <div v-if="tab == 'base'">
      base
    </div>
  </div>
</template>

<script>
// import axios from 'axios'

export default {
  name: 'Pokemon',
  created() {
    // axios
    //   .get(`https://pokeapi.co/api/v2/pokemon/${this.$route.params.name}`)
    //   .then(res => this.pokemon = res.data)
  },
  data(){
    return {
      pokemon: {},
      tab: 'about'
    }
  }
}
</script>
