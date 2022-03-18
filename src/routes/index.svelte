<script>
    import {pokemon} from "../stores/pokestore"
    import PokemanCard from "../components/pokemanCard.svelte";

    let searchTerm = ''
    let filteredPokemon = []

    $: {
        if(searchTerm) 
        {
            filteredPokemon = $pokemon.filter(pokeman => pokeman.name.toLowerCase().includes(searchTerm.toLowerCase()))
        }
        else 
        {
            filteredPokemon = [...$pokemon]
        }
    }
</script>

<svelte:head>
    <title>Pokedex</title>
</svelte:head>

<h1 class="text-4xl text-center my-8 uppercase">Svelte Kit Pokedex</h1>
<input type="text" placeholder="Search pokemon" class="w-full rounded-md text-lg p-4 border-2 border-gray-200" bind:value={searchTerm}>
<div class="grid gap-4 md:grid-cols-2 grid-cols-1 py-4">
    {#each filteredPokemon as pokeman}
        <PokemanCard {pokeman} />
    {/each}
</div>


