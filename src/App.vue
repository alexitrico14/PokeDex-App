<script>
import axios from 'axios';
import PokemonCard from './components/PokeCard.vue';

export default {
  components: {
    PokemonCard,
  },
  data() {
    return {
      pokemons: [],
    };
  },
  computed: {
    discoveredCount() {
      return this.pokemons.filter(pokemon => pokemon.discovered).length;
    },
  },
  mounted() {
    axios.get('https://pokeapi.co/api/v2/pokemon?limit=151')
      .then(response => {
        this.pokemons = response.data.results.map(pokemon => ({
          ...pokemon,
          discovered: false,
        }));
      })
      .catch(error => console.error("Error fetching Pokémon data:", error));
  },
  methods: {
    increaseDiscoveredCount(index) {
      this.pokemons[index].discovered = true;
    },
  },
};
</script>

<template>
  <div id="app" class="container d-flex flex-column align-items-center">
    <img class="pokemon-logo" alt="Vue logo" src="./assets/pokemon-logo.svg" />
    <h1>¿Quién es ese Pokémon?</h1>
    <p>Pokémones descubiertos: {{ discoveredCount }}</p>
    <section class="pokedex">
      <img class="pokedex__img" src="./assets/pokedex-top.png" alt="">
      <div class="pokemon-container d-flex  justify-content-center py-3">
        <PokemonCard v-for="(pokemon, index) in pokemons" :key="index" :pokemon="pokemon" :index="index"
          @pokemonDiscovered="increaseDiscoveredCount" />
      </div>
      <img class="pokedex__img" src="./assets/pokedex-bottom.png" alt="">
    </section>
  </div>
</template>

<style>
.pokemon-container {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.pokemon-logo {
  width: 50%;
}

.pokedex {
  background: linear-gradient(139deg, #E6F1F1, #B2D4D5);
  box-shadow: 0px 0px 20px 6px rgb(105 3 3 / 51%);
  border: 10px solid #641629;
  border-radius: 15px;
}

.pokedex__img {
  width: 100%;
}
</style>
