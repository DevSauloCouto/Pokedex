<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <h2 class="is-size-1 box title" id="h2-title">Pokedex</h2>
      <form action="" class="box" id="form-search" @submit="handleSubmit($event)">
        <div class="field">
          <label for="searchPoke" class="label">
            <div class="control">
              <p id="label-search">Search:</p>
              <input type="text" id="searchPoke" class="input" v-model="searchPokemon" placeholder="Buscar Pokemon">
            </div>
          </label>
          <button id="btn-search" class="button is-normal is-primary is-outlined">Search</button>
        </div>
      </form>
      <div v-if="filterPokemons">
        <div v-for="(poke,index) in filterPokemons" :key="poke.url">
          <Pokemon :name="poke.name" :url="poke.url" :idx="index+1" />
        </div>      
      </div>
      <div v-else>
        <div class="box">
          <h3 class="title is-size-1">Nenhum encontrado :(</h3>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios"
import Pokemon from "./components/Pokemon.vue"
export default {
  name: 'App',
  data() {
    return {
      searchPokemon: '',
      pokemons: [],
    }
  },

  async created() {
    try {
      const pokemonsResponse = await axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0");
      this.pokemons = pokemonsResponse.data.results;
    } catch (e) {
      console.log(e);
    }
  },

  components: {
    Pokemon
  },

  methods: {
    handleSubmit($event){
      $event.preventDefault();
    }
  },

  computed: {
    filterPokemons(){
      if(/^\s*$/.test(this.searchPokemon)){
        return this.pokemons;
      } else {
        const filters = this.pokemons.filter((poke) => {
          return poke.name.slice(0, this.searchPokemon.trim().length) === this.searchPokemon.trim().toLowerCase();
        });

        return filters[0] === undefined ? false : filters;
      }
    }
  }

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

#h2-title {
  background-color: #26B5B5;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  text-align: left;
}

#form-search {
  background-color: #263838;
}

#label-search {
  text-align: left;
  color: white
}

#btn-search {
  margin-top: 1%;
}
</style>
