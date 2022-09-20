<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <img src="./assets/pngegg.png">
      <hr>
      <h1 class="title">Pokedex</h1>
      <h2 class="subtitle">Developed with Vue.js and PokeAPI</h2>
      <hr>
      <input type="text" placeholder="Buscar Pokemon pelo Nome" v-model="busca" class="input is-rounded">
      <button class="button is-fullwidth is-link is-rounded" id="buscaBtn" @click="buscar">Buscar</button>
      <hr>
      <div v-for="poke in filteredPokemons" :key="poke.url">
        <PokemonLista :name="poke.name" :url="poke.url" :num="poke.id"/>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import PokemonLista from './components/PokemonLista.vue'

export default {
  name: 'App',
  data() {
    return {
      pokemons: [],
      filteredPokemons: [],
      busca: ''
    }
  },
  created: function(){
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(res => {
      this.pokemons = res.data.results;
      this.filteredPokemons = res.data.results;
    })
  },
  components: {
    PokemonLista
  },
  methods: {
    buscar: function() {
      this.filteredPokemons = this.pokemons;
      if(this.busca == '' || this.busca == ' ') {
        this.filteredPokemons = this.pokemons;
      } else {
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name == this.busca);
      }
    }

  },
  computed: {/*
    resultadoBusca: function() {
      if(this.busca == '' || this.busca == ' ') {
        return this.pokemons;
      } else {
        return this.pokemons.filter(pokemon => pokemon.name == this.busca);
      }
    }
  */}
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#buscaBtn {
  margin-top: 3%;
}
</style>
