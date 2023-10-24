<script lang="ts">
    import { PokemonClient } from 'pokenode-ts';

    let setPokemonName: string = "charizard";
    let pokemonName: string = "";
    let sprite: string | null | undefined = "";
    let id: number | null = null;
    let base_experience: number | null = null;
    let height: number | null = null;
    let weight: number | null = null;
    let abilities: string[] | null = null;
    
    async function getPokemon(name: string)
    {
        const api = new PokemonClient();

        await api
            .getPokemonByName(name)
            .then((data) => {
                pokemonName = data.name;
                sprite = data.sprites.other?.["official-artwork"].front_default;
                id = data.id;
                base_experience = data.base_experience;
                height = data.height;
                weight = data.weight;
                abilities = data.abilities.map((ability) => {
                    return ability.ability.name;
                })
            }) // will output "Luxray"
            .catch((error) => console.error(error));
    };
</script>

<div>
    <h1>PokeWebApp</h1>
    <label for="enterName">Enter name</label>
    <input id="enterName" type="text" bind:value={setPokemonName}/>
    {#await getPokemon(setPokemonName)}
        <p>Loading...</p>
    {:then} 
        <img src={sprite} alt="pokemon"/>
        <p>ID: {id}</p>
        <p>Name: {pokemonName}</p>
        <p>Base Experience: {base_experience}</p>
        <p>Height: {height}</p>
        <p>Weight: {weight}</p>
        <p>Abilities: {abilities}</p>
    {/await}
</div>