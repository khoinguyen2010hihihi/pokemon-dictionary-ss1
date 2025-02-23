<script setup>
import { fetchAPI } from '../../utils';
import { ref, onMounted } from 'vue';

const props = defineProps({
  pokemon: Object,
  pokemonId: Number
})

const pokemonTypes = ref([])

const getPokemonTypes = async (url) => {
  const data = await fetchAPI(url)
  return data.types.map(typeInfo => typeInfo.type.name)
}

onMounted(async () => {
  pokemonTypes.value = await getPokemonTypes(props.pokemon.url)
})
</script>

<template>
  <div class="item">
    <div class="item__id">
      #{{ pokemonId }}
    </div>
    <div class="item__image"
      :style="`background-image: url('https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${pokemonId}.png')`">
    </div>
    <div class="item__name">
      {{ pokemon.name }}
    </div>

    <div class="pokemon-types">
      <div v-for="type in pokemonTypes" :key="type" :class="['pokemon-type', type]">
        {{ type }}
      </div>
    </div>
  </div>
</template>

<style scoped>
.item {
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 100%;
  width: 100%;
  line-height: 24px;
  padding: 10px 5px;
  border-radius: 10px;
  box-shadow: rgba(0, 0, 0, 0.05) 0px 6px 24px 0px, rgba(0, 0, 0, 0.08) 0px 0px 0px 1px;
  transition: all .4s ease-in;
  cursor: pointer;
  background-repeat: no-repeat;
  background-size: cover;
}

.item:hover {
  box-shadow: rgba(17, 17, 26, 0.1) 0px 4px 16px, rgba(17, 17, 26, 0.1) 0px 8px 24px, rgba(17, 17, 26, 0.1) 0px 16px 56px;
}

.item__id {
  color: #2c3e50;
  font-size: 15px;
  line-height: 24px;
  text-align: center;
}

.item__image {
  width: 100%;
  aspect-ratio: 1 / 1;
  background-size: contain;
  padding: 179.913px 0px 0px;
  background-repeat: no-repeat;
}

.item__name {
  font-size: 17.55px;
  font-weight: 700;
  line-height: 28.08px;
  text-align: center;
  text-transform: capitalize;
}

.pokemon-types {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
}

.pokemon-type {
  font-size: 13px;
  border-radius: 5px;
  font-weight: 500;
  line-height: 20.8px;
  margin: 4px 3px;
  padding: 3px 4px;
  text-transform: capitalize;
  font-weight: 600;
}

.normal {
  background-color: #a8a77a;
}

.fire {
  background-color: #ff421c;
}

.water {
  background-color: #6390f0;
}

.grass {
  background-color: #78cd54;
}

.electric {
  background-color: #f7d02c;
}

.ice {
  background-color: #96d9d6;
}

.fighting {
  background-color: #c22e28;
}

.poison {
  background-color: #a33ea1;
}

.ground {
  background-color: #e2bf65;
}

.flying {
  background-color: #a98ff3;
}

.psychic {
  background-color: #f95587;
}

.bug {
  background-color: #a6b91a;
}

.rock {
  background-color: #b6a136;
}

.ghost {
  background-color: #735797;
}

.dragon {
  background-color: #6f35fc;
}

.dark {
  background-color: #705746;
}

.steel {
  background-color: #b7b7ce;
}

.fairy {
  background-color: #d685ad;
}
</style>
