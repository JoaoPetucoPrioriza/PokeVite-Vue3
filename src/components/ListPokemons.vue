<script setup>
const pokemon = defineProps(["name", "urlBaseSvg", "types"]);
const typeColors = {
  normal: 'rgba(150,118,19,0.6)', 
  bug: 'rgba(151,255,0,0.6)',
  water: 'rgba(0,172,255,0.6)',
  fire: 'rgba(255,0,0,0.75)',
  grass: 'rgba(0,255,115,0.6)',
  poison: 'rgba(115,0,255,0.6)',
  electric: 'rgba(241,255,0,0.6)',
  ground: 'rgba(163,62,23,0.6)',
  fairy: 'rgba(209,0,255,0.75)',
  fighting: 'rgba(73,55,39,0.6)',
  psychic: 'rgba(48,25,119,0.6)',
  rock: 'rgba(148,137,137,0.7)',
  ghost: 'rgba(205,186,232,0.55)',
  ice: 'rgba(25,125,185,0.55)',
  dragon: 'rgba(252,76,0,0.52)',
};
const getTypeColor = () => {
  const type = pokemon.types && pokemon.types.length > 0 ? pokemon.types[0].type.name : 'default';
  return typeColors[type] || ''; 
};
console.log(pokemon)
</script>

<template>
    <div class="col-4 col-md-6 col-xl-4">

        <div 
        class="card p-2 mb-3" 
        :style="{ backgroundColor: getTypeColor(pokemon.types)}" >
            <p class="text-center">{{ pokemon.name }}</p>
            <img :src="urlBaseSvg" class="card-img-top" alt="..." height="80" />
        </div>
    </div>
</template>


<style scoped>
.cardListPokemon {
    background: rgb(72, 63, 251);
    background: radial-gradient(circle, rgba(72, 63, 251, 0.6) 0%, rgba(70, 252, 244, 0.2) 100%);
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