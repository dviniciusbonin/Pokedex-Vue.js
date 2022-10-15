<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <img
        class="img"
        src="https://img.ibxk.com.br/2019/09/30/30091641838086.jpg?w=1120&h=420&mode=crop&scale=both"
        alt="logo"
      />
      <hr />
      <h2 class="is-size-2 is-flex is-justify-content-center">Pokedex</h2>

      <input
        id="input"
        class="input is-rounded is-info mb-4 ml-2 "
        type="text"
        placeholder="Buscar pokemon pelo nome"
        v-model="search"
      />

      <button
        id="button"
        class="button mb-2 ml-2 large-button is-success large-button"
        @click="filter"
      >
        Buscar
      </button>
      <div v-for="(poke, index) in filteredPokemons" :key="poke.url">
        <Pokemon :name="poke.name" :url="poke.url" :num="index + 1" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Pokemon from "./components/Pokemon";

export default {
  name: "App",
  components: {
    Pokemon,
  },
  data() {
    return {
      pokemons: [],
      filteredPokemons: [],
      search: "",
    };
  },
  created: function() {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=152&offset=0")
      .then((resp) => {
        this.pokemons = resp.data.results;
        this.filteredPokemons = resp.data.results;
      });
  },
  methods: {
    filter: function() {
      this.filteredPokemons = this.pokemons;
      if (this.search == "" || this.search == " ") {
        this.filteredPokemons = this.pokemons;
      } else {
        this.filteredPokemons = this.pokemons.filter(
          (pokemon) => pokemon.name == this.search.toLowerCase()
        );
      }
    },
  },
  computed: {
    resultSearch: function() {
      if (this.search == "" || this.search == " ") {
        return this.pokemons;
      } else {
        return this.pokemons.filter(
          (pokemon) => pokemon.name == this.search.toLowerCase()
        );
      }
    },
  },
};
</script>

<style>
#input {
  width: 70%;
}

#button {
  width: 25%;
}

.img {
  border-radius: 2em;
}
</style>
