<script setup>
const pokemon = defineProps(["name", "xp", "height", "img", "loading", "types"]);
import 'animate.css';

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
console.log("Tipo do Pokémon:", pokemon.types);
</script>

<template>
    <div class="card cardPokemonSelected" :class="loading ? '' : 'animate__animated animate__flipInY'"
        :style="{ backgroundColor: getTypeColor(pokemon.types) }">

        <img v-if="pokemon.name" :src="pokemon.img" class="card-img-top pt-2" :alt="pokemon.name">

        <img v-else src="../assets/egg_pokemon.svg" class="card-img-top pt-2" alt="Pokemon Egg">



        <div class="card-body">
            <h5 class="card-title text-center">{{ pokemon.name || '???' }}</h5>
            <hr>

            <div class="row">
                <section class="col">
                    <strong>XP:</strong>
                    <span>{{ pokemon.xp }}</span>
                </section>
                <section class="col">
                    <strong>Altura:</strong>
                    <span>{{ pokemon.height }}</span>
                </section>
            </div>

            <!-- <div class="d-none d-md-block">
                <div class="row mt-4">
                    <section class="col">
                        <strong>XP:</strong>
                        <span>{{ pokemon.xp }}</span>
                    </section>
                    <section class="col">
                        <strong>Altura:</strong>
                        <span>{{ pokemon.height }}</span>
                    </section>
                </div>
            </div> -->
        </div>
    </div>
</template>

<style scoped>
.cardPokemonSelected {
    height: 75vh;
    overflow: hidden;
    transition: background-color 0.3s ease;
}

.cardPokemonSelected img {
    height: 250px;
}


@media (max-width: 768px) {
    .cardPokemonSelected {
        height: 30vh;
        width: 40%;
        margin: 0 auto 10px auto;
    }

    .cardPokemonSelected img {
        height: 100px;
    }
}
</style>