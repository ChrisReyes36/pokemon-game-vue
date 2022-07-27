<template>
  <h1 v-if="!pokemon">Espere por favor...</h1>
  <div v-else>
    <h1>¿Quién es este pokémon?</h1>
    <!-- picture -->
    <PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon" />
    <!-- actions -->
    <PokemonOptions :pokemons="pokemonArr" @selection-pokemon="checkAnswer" />

    <template v-if="showAswer" class="fade-in">
      <h2>{{ message }}</h2>
      <button style="cursor: pointer" @click="newGame">Nuevo Juego</button>
    </template>
  </div>
</template>

<script>
import PokemonPicture from "@/components/PokemonPicture.vue";
import PokemonOptions from "@/components/PokemonOptions.vue";
import getPokemonOptions from "@/helpers/getPokemonOptions";

export default {
  name: "PokemonPage",
  components: {
    PokemonPicture,
    PokemonOptions,
  },
  data() {
    return {
      pokemonArr: [],
      pokemon: null,
      showPokemon: false,
      showAswer: false,
      message: "",
    };
  },
  methods: {
    async mixPokemonArray() {
      this.pokemonArr = await getPokemonOptions();

      const rndInt = Math.floor(Math.random() * 4);
      this.pokemon = this.pokemonArr[rndInt];
    },
    checkAnswer(selectedId) {
      this.showPokemon = true;
      this.showAswer = true;
      if (selectedId === this.pokemon.id) {
        this.message = `¡Correcto, ${this.pokemon.name}!`;
      } else {
        this.message = `¡Ops, era ${this.pokemon.name}!`;
      }
    },
    newGame() {
      this.pokemonArr = [];
      this.pokemon = null;
      this.showPokemon = false;
      this.showAswer = false;
      this.message = "";
      this.mixPokemonArray();
    },
  },
  mounted() {
    this.mixPokemonArray();
  },
};
</script>

<style>
</style>