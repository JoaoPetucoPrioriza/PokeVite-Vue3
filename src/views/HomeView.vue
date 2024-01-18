<script setup>
import { onMounted, reactive, ref, computed } from 'vue';
import ListPokemons from '@/components/ListPokemons.vue';
import cardPokemonSelected from '@/components/cardPokemonSelected.vue';

const pokemonList = ref([]);

let pokemons = reactive(ref());
let urlBaseSvg = ref("https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/");
let searchPokemonField = ref("");
let pokemonSelected = reactive(ref());
let loading = ref(false);

onMounted(() => {
  fetch('https://pokeapi.co/api/v2/pokemon?limit=151&offset=0')
    .then(res => res.json())
    .then(async (res) => {  pokemons.value = res.results;
      const pokemonDetailsPromises = res.results.map(async (pokemon) => {
        const response = await fetch(pokemon.url);
        const data = await response.json();
        console.log('Data do Pokémon lista:', data);
        console.log('Tipos do Pokémon:', data.types);
        return {
          name: data.name,
          urlBaseSvg: data.sprites.front_default,
          types: data.types,
        };
      });

      pokemonList.value = await Promise.all(pokemonDetailsPromises);
    })
    .catch(error => console.error('Error fetching Pokémon list:', error));
});

const pokemonsFiltered = computed(() => {
  if (pokemons.value && searchPokemonField.value) {
    return pokemons.value.filter(pokemon =>
      pokemon.name.toLowerCase().includes(searchPokemonField.value.toLowerCase())
    )
  }
  return pokemons.value;
});

const selectPokemon = async (pokemon) => {
  loading.value = true;
  try {
    const response = await fetch(pokemon.url);
    const data = await response.json();
    pokemonSelected.value = {
      name: data.name,
      base_experience: data.base_experience,
      height: data.height,
      sprites: data.sprites,
      types: data.types,
    };


    const speciesResponse = await fetch(data.species.url);
    const speciesData = await speciesResponse.json();

    const evolutionChainUrl = speciesData.evolution_chain.url;

    const evolutionChainResponse = await fetch(evolutionChainUrl);
    const evolutionChainData = await evolutionChainResponse.json();

    const evolutions = parseEvolutions(evolutionChainData.chain);

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

  return evolutions.slice(1);
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
              :urlBaseSvg="urlBaseSvg + pokemon.url.split('/')[6] + '.svg'" 
              @click="selectPokemon(pokemon)" 
              :types="pokemonSelected?.types || []"
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
