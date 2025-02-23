<script setup>
import { ref, computed } from 'vue'
import PokemonCard from './components/PokemonCard.vue'
import PokemonPopup from './components/PokemonPopup.vue';
import { fetchAPI, getIDPokemon } from '../utils/index.js'

let pokemons = [];
const offset = ref(0);
const NUMBER_OF_RENDER = 36;
const filteredPokemons = ref([]);
const selectedPokemon = ref(null);
const pokemonTypes = ref([]);
const flavorText = ref('');
const pokemonDetails = ref(null);

const renderPokemons = computed(() => filteredPokemons.value.slice(0, offset.value + NUMBER_OF_RENDER));

async function getPokemon() {
  const data = await fetchAPI('https://pokeapi.co/api/v2/pokemon/?offset=0&limit=898');
  pokemons = data.results;
  filteredPokemons.value = pokemons;
}

getPokemon();

function handleLoadMore() {
  offset.value += NUMBER_OF_RENDER;
}

function handleSearch(event) {
  filteredPokemons.value = pokemons.filter((pokemon) => {
    return pokemon.name.includes(event.target.value);
  });
  offset.value = 0;
}

const getPokemonTypes = async (url) => {
  const data = await fetchAPI(url)
  return data.types.map(typeInfo => typeInfo.type.name)
}

const getFlavorText = async (pokemonName) => {
  const data = await fetchAPI(`https://pokeapi.co/api/v2/pokemon-species/${pokemonName}`)
  const englishText = data.flavor_text_entries.find(entry => entry.language.name === 'en');
  return englishText ? englishText.flavor_text : '';
}

const getPokemonDetails = async (url) => {
  const data = await fetchAPI(url);
  return {
    height: data.height,
    weight: data.weight,
    abilities: data.abilities.map(ability => ability.ability.name),
    stats: data.stats.map(stat => ({
      name: stat.stat.name,
      value: stat.base_stat
    }))
  };
}

const getEvolutionChain = async (pokemonName) => {
  const speciesData = await fetchAPI(`https://pokeapi.co/api/v2/pokemon-species/${pokemonName}`);
  const evolutionChainUrl = speciesData.evolution_chain.url;
  const evolutionChainData = await fetchAPI(evolutionChainUrl);
  return evolutionChainData;
}

async function handlePokemonClick(pokemon) {
  selectedPokemon.value = pokemon;
  pokemonTypes.value = await getPokemonTypes(pokemon.url);
  flavorText.value = await getFlavorText(pokemon.name);
  pokemonDetails.value = await getPokemonDetails(pokemon.url);
  const evolutionChain = await getEvolutionChain(pokemon.name);
  selectedPokemon.value.evolutionChain = extractEvolutionChain(evolutionChain.chain);
}

function extractEvolutionChain(chain) {
  const evolutionChain = [];
  let currentStage = chain;

  while(currentStage) {
    evolutionChain.push({
      name: currentStage.species.name,
      url: currentStage.species.url
    });

    if(currentStage.evolves_to.length > 0) {
      currentStage = currentStage.evolves_to[0];
    } else {
      currentStage = null;
    }
  }

  return evolutionChain;
}

function handleBackClick() {
  selectedPokemon.value = null;
  pokemonTypes.value = [];
  flavorText.value = '';
  pokemonDetails.value = null;
}
</script>

<template>
  <div class="container">
    <PokemonPopup
      v-if="selectedPokemon"
      :selectedPokemon="selectedPokemon"
      :pokemonId="getIDPokemon(selectedPokemon.url)"
      :pokemonTypes="pokemonTypes"
      :flavorText="flavorText"
      :pokemonDetails="pokemonDetails"
      @back="handleBackClick"
    />

    <!-- Title Section -->
    <div class="title">
      <p class="title__main">Pokemon API</p>
      <input class="title__input" type="text" placeholder="Search some Pokemon..." @input="handleSearch">
    </div>

    <div class="items">
      <PokemonCard
        v-for="item in renderPokemons"
        :key="item.name"
        :pokemon="item"
        :pokemonId="getIDPokemon(item.url)"
        @click="handlePokemonClick(item)"
      />
    </div>

    <!-- Load more button -->
    <button class="load-more" @click="handleLoadMore">Load more</button>
  </div>
</template>

<style scoped>
* {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
  font-family: "Inter", sans-serif;
}

.container {
  margin: 0 auto;
  max-width: 1200px;
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 50px;
}

.title {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 70px 0;
}

.title__main {
  color: #2c3e50;
  font-size: 37.5px;
  line-height: 60px;
  margin-bottom: 30px;
  font-weight: 400;
}

.title__input {
  width: 500px;
  padding: 20px;
  background-color: #fff;
  border-radius: 30px;
  font-family: "Arial", sans-serif;
  box-shadow: #64646f33 0px 7px 29px 0px;
  border: grey solid 1px;
}

.items {
  width: 100%;
  display: grid;
  grid-template-columns: repeat(6, 15.87%);
  gap: 10px;
  padding: 0 10px;
}

@media screen and (max-width: 1199px) {
  .items {
    display: grid;
    grid-template-columns: repeat(4, 24.375%);
  }
}

@media screen and (max-width: 956px) {
  .items {
    display: grid;
    grid-template-columns: repeat(3, 32.77%);
  }
}

@media screen and (max-width: 768px) {
  .items {
    display: grid;
    grid-template-columns: repeat(2, 49.58%);
  }
}

.load-more {
  background-color: #ff4d4f;
  border-radius: 10px;
  color: #fff;
  padding: 20px 25px;
  text-align: center;
  border: none;
  margin-top: 20px;
  margin-bottom: 20px;
  cursor: pointer;
}
</style>