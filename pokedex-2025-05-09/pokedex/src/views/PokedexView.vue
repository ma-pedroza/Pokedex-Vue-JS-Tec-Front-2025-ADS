<template>
    <div>
        <h1 class="text-3xl font-bold text-center mb-6">
            Pokedex
        </h1>

        <div class="grid grid-cols-2 md:grid-cols-4 lg:grid-cols-6 gap-4">
            <PokemonCard v-for="p in pokemons" :key="p.name" :pokemon="p"></PokemonCard>
        </div>

        <div class="flex justify-center gap-4 mt-6">
            <button @click="previousPage"
                class="btn bg-blue-600 text-white px-4 py-2 rounded hover:bg-blue-700 disabled:opacity-50"
                :disabled="offset == 0">Anterior</button>
            <button @click="nextPage"
                class="btn bg-blue-600 text-white px-4 py-2 rounded hover:bg-blue-700">Próximo</button>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import PokemonCard from '../components/PokemonCard.vue'

const pokemons = ref([])
const offset = ref(0)
const limit = 12

const fetchPokemons = async () => {
    const response = await axios.get(`https://pokeapi.co/api/v2/pokemon?offset=${offset.value}&limit=${limit}`)
    const results = response.data.results

    console.log(results)

    const detailed = await Promise.all(
        results.map(pokemon => axios.get(pokemon.url).then(response => response.data))
    )
    pokemons.value = detailed.map(p => ({
        name: p.name,
        type: p.types[0].type.name,
        image: p.sprites.front_default
    }))


    console.log(pokemons)

    console.log(detailed)
}

const nextPage = () => {
    offset.value += limit
    fetchPokemons()
}

const previousPage = () => {
    if (offset.value >= limit) {
        offset.value -= limit
        fetchPokemons()
    }
}

onMounted(fetchPokemons)
</script>
