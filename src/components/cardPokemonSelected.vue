<script setup>
const pokemon = defineProps(["name", "xp", "height", "img", "loading", "types", "evolutions"]);

import 'animate.css';

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
    const type = pokemon.types && pokemon.types.length > 0 ? pokemon.types[0].type.name : 'default';
    return typeColors[type] || '';
};

</script>

<template>
    <div class="card cardPokemonSelected" :class="loading ? '' : 'animate__animated animate__zoomIn'"
        :style="{ backgroundColor: getTypeColor(pokemon.types) }">

        <img v-if="pokemon.name" :src="pokemon.img" class="card-img-top pt-2" :alt="pokemon.name">

        <img v-else src="../assets/egg_pokemon.svg" class="card-img-top pt-2" alt="Pokemon Egg">


        <div class="card-body">
            <h5 class="card-title text-center fw-bold">{{ pokemon.name || '???' }}</h5>
            <hr>

            <div class="row fw-bold">
                <section class="col">
                    <strong>XP: </strong>
                    <span>{{ pokemon.xp }}</span>
                </section>
                <section class="col">
                    <strong>Altura: </strong>
                    <span>{{ pokemon.height }}</span>
                </section>
                <section class="col">
                    <strong>Classe: </strong>
                    <span>{{ pokemon.types && pokemon.types.length > 0 ? pokemon.types[0].type.name : '???' }}</span>
                </section>
            </div>

            <div>


                <div class="fw-bold" v-if="evolutions && evolutions.length > 0">
                   <br><br> <h5 class="fw-bold">Evoluções:</h5>
                    <ul class="list-group">
                        <li class="list-group-item listDesign" v-for="evolution in evolutions" :key="evolution.name">
                            {{ evolution.name }}
                        </li>
                    </ul>
                </div>
                <div v-else>
                    <p>Nenhuma evolução disponível.</p>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>

.listDesign{
    background-color: transparent;
    border: none;
    border-bottom: 1px solid white;
}
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
        height: 40vh;
        width: 40%;
        margin: 0 auto 10px auto;
    }

    .cardPokemonSelected img {
        height: 100px;
    }
}
</style>