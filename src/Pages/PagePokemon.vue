<template>
  <h1 v-if="!pokemon">Espere por favor...</h1>
  <div v-else>
    <h1>Quien es ese Pokemón?</h1>
    <ImagenPokemon :pokemonId="pokemon.id" :showPokemon="showPokemon" />
    <OpcionesPokemon :pokemons="pokemonArr" @selectionPokemon="revisarRespuesta" />
  </div>

  <div v-if="showAnswer">
    <h2 class="fade-in">{{ message }}</h2>
    <button class="btnPokemon" @click="nuevoJuego">Nuevo Juego</button>
  </div>
</template>

<script>
import ImagenPokemon from '@/components/ImagenPokemon';
import OpcionesPokemon from '@/components/OpcionesPokemon';

import getPokeOpciones from '@/helpers/getPokeOpciones';

export default {
  name: 'PagePokemon',

  data() {
    return {
      pokemonArr: [],
      pokemon: null,
      showPokemon: false,
      showAnswer: false,
      message: null,
    };
  },

  methods: {
    async mixPokemonArray() {
      this.pokemonArr = await getPokeOpciones();
      const rndInt = Math.floor(Math.random() * 4);
      this.pokemon = this.pokemonArr[rndInt];
    },

    revisarRespuesta(selectId) {
      this.showPokemon = true;
      this.showAnswer = true;
      if (this.pokemon.id === selectId) {
        this.message = `Correcto, ${this.pokemon.name}`;
      } else {
        this.message = `Incorrecto..., era ${this.pokemon.name}`;
      }
    },

    nuevoJuego() {
      (this.pokemonArr = []), (this.pokemon = null), (this.showPokemon = false), (this.showAnswer = false), (this.message = null), this.mixPokemonArray();
    },
  },

  mounted() {
    this.mixPokemonArray();
  },

  components: {
    ImagenPokemon,
    OpcionesPokemon,
  },
};
</script>

<style scoped>
.btnPokemon {
  border-radius: 1em;
  cursor: pointer;
  font-size: 15px;
  outline: none;
  padding: 1em;
}

.btnPokemon:hover {
  background-color: black;
  color: white;
}
</style>
