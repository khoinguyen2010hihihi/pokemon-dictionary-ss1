<script setup>
  import { getIDPokemon } from '../../utils';

  defineProps({
    selectedPokemon: {
      type: Object,
      required: true
    },
    pokemonId: {
      type: Number,
      required: true,
    },
    pokemonTypes: {
      type: Array,
      required: true,
    },
    flavorText: {
      type: String,
      required: true,
    },
    pokemonDetails: {
      type: Object,
      required: true,
    }
  })

  const emit = defineEmits(['back']);
  function handleBackClick() {
    emit('back');
  }
</script>

<template>
  <div class="popup"> 
    <div class="wrapper">
      <button class="back-btn" @click="handleBackClick">
        Back
      </button>

      <div class="wrapper__content">
        <div class="col-full">
          <div class="item__image"
            :style="`background-image: url('https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${pokemonId}.png')`">
          </div>
          
          <div class="pokemon-types">
            <div v-for="type in pokemonTypes" :key="type" :class="['pokemon-type', type]">
              {{ type }}
            </div>
          </div>
          
          <h2>{{ selectedPokemon.name }}</h2>

          <p class="entry">
            {{ flavorText }}
          </p>

          <div class="detail__wrap">
            <div>
              <h3 class="label">Height</h3>
              <div class="detail-tag">{{ pokemonDetails?.height }}</div>
            </div>
            <div>
              <h3 class="label">Weight</h3>
              <div class="detail-tag">{{ pokemonDetails?.weight }}</div>
            </div>
          </div>

          <div class="ability">
            <h3 class="label">Abilities</h3>
            <div class="detail__wrap">
              <div v-for="ability in pokemonDetails?.abilities || []" :key="ability" class="detail-tag">
                {{ ability }}
              </div>
            </div>
          </div>

          <div class="stats">
            <h3 class="label">Stats</h3>
            <div class="detail__wrap">
              <div class="tag">
                <div class="tag__label HP">HP</div>
                <div class="tag__detail">{{ pokemonDetails?.stats[0].value }}</div>
              </div>
              <div class="tag">
                <div class="tag__label ATK">ATK</div>
                <div class="tag__detail">{{ pokemonDetails?.stats[1].value }}</div>
              </div>
              <div class="tag">
                <div class="tag__label DEF">DEF</div>
                <div class="tag__detail">{{ pokemonDetails?.stats[2].value }}</div>
              </div>
              <div class="tag">
                <div class="tag__label SpA">SpA</div>
                <div class="tag__detail">{{ pokemonDetails?.stats[3].value }}</div>
              </div>
              <div class="tag">
                <div class="tag__label SpD">SpD</div>
                <div class="tag__detail">{{ pokemonDetails?.stats[4].value }}</div>
              </div>
              <div class="tag">
                <div class="tag__label SPD">SPD</div>
                <div class="tag__detail">{{ pokemonDetails?.stats[5].value }}</div>
              </div>
            </div>
          </div>

          <div class="evolution">
            <h3 class="label">Evolution</h3>
            <div class="evolution__wrap">
              <div v-for="evolution in selectedPokemon.evolutionChain" :key="evolution.name" class="evolution-item">
                <h4>{{ evolution.name }}</h4>
                <div class="evolution-item__image"
                  :style="`background-image: url('https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${getIDPokemon(evolution.url)}.png')`">
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
.popup {
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  background-color: #fff;
  display: flex;
  justify-content: center;
  z-index: 1000;
}

.popup .wrapper {
  max-width: 1200px;
  margin: 50px;
  width: 1200px;
}

.popup .wrapper__content {
  display: flex;
  flex-wrap: wrap;
}

.popup .col-full {
  max-width: 500px;
  width: 100%;
  text-align: center;
  margin: auto;
}

.popup .back-btn {
  position: absolute;
  top: 30px;
  left: 70px;
  background-color: #fff;
  color: #000;
  box-shadow: #63636333 0 4px 8px;
  border: none;
  padding: 10px 30px;
  border-radius: 25px;
  cursor: pointer;
}

.popup h2 {
  white-space: nowrap;
  font-weight: 700;
  text-align: center;
  text-transform: capitalize;
  /* width: 100%; */
}

.popup p {
  margin-bottom: 10px;
}

.popup .item__image {
  width: 150px;
  height: 150px;
  margin: auto;
  background-size: cover;
}

.popup .pokemon-types {
  display: flex;
  justify-content: center;
  width: 100%;
}

.popup .pokemon-type {
  font-weight: 600;
  text-align: center;
  margin-bottom: 5px;
  padding: 3px 4px;
  border-radius: 5px;
  margin: 0 3px;
  font-size: 13px;
  font-weight: 500;
  text-transform: capitalize;
  box-shadow: #0000000d 0 6px 24px, #00000014 0 0 0 1px;
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

.popup .detail__wrap {
  display: flex;
  justify-content: space-evenly;
  margin-bottom: 5px;
  width: 100%;
}

.popup .label {
  font-weight: 600;
  text-align: center;
  margin-bottom: 5px;
}

.popup .detail-tag {
  width: 100px;
  margin: 5px;
  padding: 2px;
  border-radius: 30px;
  background-color: #f6f8fc;
  text-align: center;
  text-transform: capitalize;
}

.HP {
    background-color: #df2140
}

.ATK {
    background-color: #ff994d
}

.DEF {
    background-color: #eecd3d
}

.SpA {
    background-color: #85ddff
}

.SpD {
    background-color: #96da83
}

.SPD {
    background-color: #fb94a8
}

.popup .tag {
  margin: 5px;
  padding: 5px;
  background-color: #f6f8fc;
  border-radius: 30px;
  box-shadow: #63636333 0 2px 8px;
}

.popup .tag__label {
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 50%;
  width: 25px;
  height: 25px;
  font-size: 10px;
  font-weight: 700;
  color: #fff;
  text-transform: uppercase;
}

.popup .tag__detail {
  text-align: center;
}

.popup .evolution {
  width: 100%;
}

.popup .evolution__wrap {
  display: flex;
  justify-content: center;
}

.popup .evolution-item {
  display: flex;
  justify-content: space-around;
  align-items: center;
}

.popup .evolution-item__image {
  height: 96px;
  width: 96px;
}

.popup h4 {
  white-space: nowrap;
  font-weight: 600;
  text-align: center;
  text-transform: capitalize;
}
</style>