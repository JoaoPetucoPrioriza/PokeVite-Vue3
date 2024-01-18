<script setup>
const pokemon = defineProps(["name", "urlBaseSvg", "types"]);
const typeColors = {
    normal: 'rgba(189,183,107,0.7)', // Cáqui escuro
    bug: 'rgba(173,255,47,0.6)', // Verde amarelado
    water: 'rgba(135,206,250,0.6)', // Azul céu
    fire: 'rgba(255,69,0,0.7)', // Laranja avermelhado
    grass: 'rgba(34,139,34,0.6)', // Verde grama
    poison: 'rgba(186,85,211,0.6)', // Roxo orquidea
    electric: 'rgba(255,255,51,0.7)', // Amarelo claro 4
    ground: 'rgba(160,82,45,0.7)', // Siena 
    fairy: 'rgba(255,105,180,0.75)', // HotPink
    fighting: 'rgba(72,61,139,0.8)', // Azul Ardósia Escuro
    psychic: 'rgba(138,43,226,0.7)', // Violeta Azul
    rock: 'rgba(192,192,192,0.7)', // Silver
    ghost: 'rgba(216,191,216,0.55)', // Cardo
    ice: 'rgba(240,248,255,0.45)', // Alice Blue
    dragon: 'rgba(255,99,71,0.7)', // Tomato
};

const getTypeColor = () => {
  console.log('Tipos do Pokémon em ListPokemons:', pokemon.types); // Adicione este log
  const pokemonTypes = pokemon.types || [];

  if (pokemonTypes.length > 0) {
    const type = pokemonTypes[0].type.name.toLowerCase();
    console.log(`Tipo do Pokémon: ${type}`);
    return typeColors[type] || 'White';
  }

  console.log('Tipo do Pokémon não encontrado em ListPokemons. Tipos:', pokemonTypes);
  return 'black'; 
};

</script>

<template>
    <div class="col-4 col-md-6 col-xl-4">

        <div 
        class="card p-2 mb-3 cardListPokemon" 
        :style="{ backgroundColor: getTypeColor() }" >
            <p class="text-center">{{ pokemon.name }}</p>
            <img :src="urlBaseSvg" class="card-img-top" alt="..." height="80" />
        </div>
    </div>
</template>


<style scoped>
.cardListPokemon {
   
    cursor: pointer;
}
.cardListPokemon:hover {
    background: rgb(72, 63, 251);
    background: radial-gradient(circle, rgba(72, 63, 251, 0.6) 0%, rgba(70, 252, 244, 0.4) 100%);
}
.cardListPokemon img{
    transition: 0.5s ease;
}
.cardListPokemon img:hover {
    transform: scale(1.1);
    transition: 0.3s ease;
}

</style>