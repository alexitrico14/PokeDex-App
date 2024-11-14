<script>
import axios from 'axios';

export default {
    props: ['pokemon', 'index'],
    data() {
        return {
            userGuess: '',
            pokemonImageUrl: '',
        };
    },
    mounted() {
        axios.get(this.pokemon.url)
            .then(response => {
                this.pokemonImageUrl = response.data.sprites.front_default;
            })
            .catch(error => console.error("Error fetching Pokémon image:", error));
    },
    methods: {
        checkPokemon() {
            if (this.userGuess.toLowerCase() === this.pokemon.name.toLowerCase()) {
                this.pokemon.discovered = true;
                this.$emit('pokemonDiscovered', this.index);
            } else {
                alert("Nombre incorrecto. ¡Intenta nuevamente!");
            }
        },
    },
};
</script>

<template>
    <div class="pokemon-card d-flex flex-column align-items-center  mb-4">
        <img :src="pokemonImageUrl" :alt="pokemon.name"
            :style="{ filter: pokemon.discovered ? 'none' : 'blur(5px) grayscale(100%)' }" />
        <div v-if="!pokemon.discovered">
            <input class="form-control my-2 px-2" v-model="userGuess" type="text" placeholder="Nombre del Pokémon" />
            <button class="btn btn-outline-success" @click="checkPokemon">Descubrir</button>
        </div>
        <p v-else>{{ pokemon.name }}</p>
    </div>
</template>

<style>
.pokemon-card {
    width: 200px;
    background-color: #fff;
    text-align: center;
    border: 1px solid #b5b5b5;
    padding: 10px;
    border-radius: 8px;
    transition: box-shadow 0.7s cubic-bezier(0.18, 0.89, 0.32, 1.28);
}

.pokemon-card:hover {
    box-shadow: 7px 5px 5px 2px rgb(71 67 32 / 44%)
}

.pokemon-card img {
    width: 100px;
    height: 100px;
}
</style>