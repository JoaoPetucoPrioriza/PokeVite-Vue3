<script setup>
import { onMounted, reactive, ref, computed } from 'vue';
import ListPokemons from '@/components/ListPokemons.vue';
import cardPokemonSelected from '@/components/cardPokemonSelected.vue';


let pokemons = reactive(ref());
let urlBaseSvg = ref("https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/");
let searchPokemonField = ref("");
let pokemonSelected = reactive(ref());
let loading = ref(false);

onMounted(() => {
  fetch('https://pokeapi.co/api/v2/pokemon?limit=151&offset=0')
    .then(res => res.json())
    .then(res => pokemons.value = res.results);
})

const pokemonsFiltered = computed(() => {
  if (pokemons.value && searchPokemonField.value) {
    return pokemons.value.filter(pokemon =>
      pokemon.name.toLowerCase().includes(searchPokemonField.value.toLowerCase())
    )
  }
  return pokemons.value;
})

const selectPokemon = async (pokemon) => {
  loading.value = true;
  try {
    const response = await fetch(pokemon.url);
    const data = await response.json();
    console.log('Data do Pokémon:', data);

    pokemonSelected.value = {
      name: data.name,
      base_experience: data.base_experience,
      height: data.height,
      sprites: data.sprites,
      types: data.types,
    };

    // Adição das informações de evolução
    const speciesResponse = await fetch(data.species.url);
    const speciesData = await speciesResponse.json();
    console.log('Data da Espécie:', speciesData);

    const evolutionChainUrl = speciesData.evolution_chain.url;
    console.log('URL da Cadeia de Evolução:', evolutionChainUrl);

    const evolutionChainResponse = await fetch(evolutionChainUrl);
    const evolutionChainData = await evolutionChainResponse.json();
    console.log('Data da Cadeia de Evolução:', evolutionChainData);

    const evolutions = parseEvolutions(evolutionChainData.chain);
    console.log('Evoluções:', evolutions);

    pokemonSelected.value.evolutions = evolutions;

  } catch (error) {
    console.error(error);
    alert(error.message || 'Erro ao carregar o Pokémon.');
  } finally {
    loading.value = false;
  }
};
const parseEvolutions = (chain) => {
  const evolutions = [];

  const addEvolutionsRecursive = (stage) => {
    evolutions.push({ name: stage.species.name });

    if (stage.evolves_to.length > 0) {
      stage.evolves_to.forEach((evolution) => {
        addEvolutionsRecursive(evolution);
      });
    }
  };

  if (chain && chain.species) {
    addEvolutionsRecursive(chain);
  }

  return evolutions.slice(1); // Remove o primeiro elemento, que é o próprio Pokémon inicial
};
</script>

<template>
  <main>
    <div class="container text-center">

      <div class="row mt-4">
        <div class="col-sm-12 col-md-6">

          <cardPokemonSelected 
          class="text-body-secondary"
          :name="pokemonSelected?.name" 
          :xp="pokemonSelected?.base_experience"
          :height="pokemonSelected?.height" 
          :img="pokemonSelected?.sprites.other.dream_world.front_default" 
          :loading="loading"
          :types="pokemonSelected?.types"
          :evolutions="pokemonSelected?.evolutions"
          />
        </div>

        <div class="col-sm-12 col-md-6">
          <div class="card card-list">
            <div class="card-body row">

              <div class="mb-3">
                <label hidden for="searchPokemonField" class="form-label">
                  Pokemon
                </label>

                <input v-model="searchPokemonField" type="text" class="form-control" id="searchPokemonField"
                  placeholder="Pesquisar...">

              </div>

              <ListPokemons v-for="pokemon in pokemonsFiltered" 
              :key="pokemon.name" 
              :name="pokemon.name"
              :urlBaseSvg="urlBaseSvg + pokemon.url.split('/')[6] + '.svg'" @click="selectPokemon(pokemon)" 
              :types="pokemon.types"
              />

            </div>

          </div>
        </div>

      </div>
    </div>
  </main>
</template>

<style scoped>
.card-list{
  max-height: 75vh;
  overflow-y: scroll;
  overflow-x: hidden;
}

@media (max-width: 768px) {
  .card-list{
    max-height: 50vh;
}
}
</style>
