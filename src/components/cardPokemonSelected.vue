<script setup>
const pokemon = defineProps ( ["name", "xp", "height", "img", "loading"] )
import 'animate.css';

</script>

<template>

    <div 
    class="card cardPokemonSelected"
    :class="loading ? '' : 'animate__animated animate__flipInY'"
    >
        
        <img 
        v-if="pokemon.name"
        :src="pokemon.img" 
        height="250px"
        class="card-img-top pt-2" 
        :alt="pokemon.name"
        >
        <img 
        v-else
        src="../assets/egg_pokemon.svg" 
        height="250px"
        class="card-img-top pt-2" 
        alt="Pokemon Egg"
        >


        
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
        </div>
    </div>

</template>

<style scoped>
.cardPokemonSelected{
    height: 75vh;
    background: rgb(143,129,51);
    background: radial-gradient(circle, rgba(190, 163, 26, 0.804) 0%, rgba(255, 42, 0, 0.921) 100%);
}



</style>