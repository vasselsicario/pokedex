<template>
  <div class="list">
    <article v-for="pokemon in pokemonsFiltered" :key="pokemon.name" v-on:click="showPokemonDetailEmit(pokemon.url)">
      <h3>{{pokemon.name}}</h3>
      <img :src="image_url + pokemon.name + '.png'"/>
    </article>
  </div>
</template>

<script>
import axios from 'axios';
import _ from 'lodash';
export default {
  props: [
    "pokemon_search"
  ],
  data: () => {
    return {
      pokemons: [

      ],
      api_url: "https://pokeapi.co/api/v2/pokemon/?ofset=0&limit=-1",
      image_url: "https://img.pokemondb.net/sprites/bank/normal/"
    }
  },
  created() {
    axios.get(this.api_url)
      .then(response => {
        this.pokemons = response.data.results
      })
  },
  methods: {
    showPokemonDetailEmit(pokemon) {
      this.$emit("ShowPokemon", pokemon);
    }
  },
  computed: {
    pokemonsFiltered() {
      return _.filter(this.pokemons, (pokemon) => {
        return pokemon.name.toLowerCase().startsWith(this.pokemon_search.toLowerCase());
      });
    }
  }
};
</script>

<style lang="scss" scoped>
.list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  grid-gap: 10px;
  width: 100%;
  max-width: 510px;
}
article {
  height: 150px;
  background-color: #efefef;
  text-align: center;
  text-transform: capitalize;
  border-radius: 5px;
  cursor: pointer;
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2), 0 10px 10px rgba(0, 0, 0, 0.2);
}
h3 {
  margin: 0;
}
#scroll-trigger {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 150px;
  font-size: 2rem;
  color: #efefef;
}

img {
  width: 96px;
  height: 96px;
}
</style>

