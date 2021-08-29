<script>
    import { onMount } from 'svelte';
    import _ from 'lodash';

    let games = [];
    let name, publisher, year, genre;
    let sortDirection = 'asc';

    onMount(async () => {
        games = JSON.parse(localStorage.getItem('games') ?? "[]");
    });

    const sortName = () => {
        if (sortDirection === 'asc') {
            sortDirection = 'desc';
            games = games.sort((a, b) => a.name < b.name ? 1 : -1);
        }
        else if (sortDirection === 'desc') {
            sortDirection = 'asc';
            games = games.sort((a, b) => a.name < b.name ? -1 : 1);
        }
    }
    const sortPublisher = () => {
        if (sortDirection === 'asc') {
            sortDirection = 'desc';
            games = games.sort((a, b) => a.publisher < b.publisher ? 1 : -1);
        }
        else if (sortDirection === 'desc') {
            sortDirection = 'asc';
            games = games.sort((a, b) => a.publisher < b.publisher ? -1 : 1);
        }
    }
    const sortGenre = () => {
        if (sortDirection === 'asc') {
            sortDirection = 'desc';
            games = games.sort((a, b) => a.genre < b.genre ? 1 : -1);
        }
        else if (sortDirection === 'desc') {
            sortDirection = 'asc';
            games = games.sort((a, b) => a.genre < b.genre ? -1 : 1);
        }
    }
    const sortYear = () => {
        if (sortDirection === 'asc') {
            sortDirection = 'desc';
            games = games.sort((a, b) => a.year < b.year ? 1 : -1);
        }
        else if (sortDirection === 'desc') {
            sortDirection = 'asc';
            games = games.sort((a, b) => a.year < b.year ? -1 : 1);
        }
    }

    const removeGame = (game) => {
        _.remove(
            games,
            (g) => JSON.stringify(g) === JSON.stringify(game)
        );
        games = games;
        localStorage.setItem('games', JSON.stringify(games));
    }

    const handleSubmit = () => {
        games = [...games, {
            name: name,
            publisher: publisher,
            genre: genre,
            year: year,
        }];
        localStorage.setItem('games', JSON.stringify(games));
    }
</script>

<h1>Svelte Game Tracker</h1>
<form on:submit|preventDefault={handleSubmit}>
    <label for="name">Name</label>
    <input required id="name" bind:value={name}/>
    <label for="publisher">Publisher</label>
    <input required id="publisher" bind:value={publisher}/>
    <label for="genre">Genre</label>
    <input required id="genre" bind:value={genre}/>
    <label for="year">Year</label>
    <input required type="year" id="year" bind:value={year}/>
    <button type="submit">Add game</button>
</form>
<table>
    <tr class="table-head">
        <td on:click={sortName}>Name</td>
        <td on:click={sortPublisher}>Publisher</td>
        <td on:click={sortGenre}>Genre</td>
        <td on:click={sortYear}>Year</td>
        <td></td>
    </tr>
    {#each games as game}
        <tr>
            <td>{game.name}</td>
            <td>{game.publisher}</td>
            <td>{game.genre}</td>
            <td>{game.year}</td>
            <td><button on:click={() => removeGame(game)}>Remove</button></td>
        </tr>
    {/each}
</table>

<style>
    h1, form {
        text-align: center;
    }
    table {
        margin-top: 2em;
        margin-left: auto;
        margin-right: auto;
    }
    .table-head > td {
        font-weight: bold;
        padding-bottom: 1em;
    }
    td {
        min-width: 6em;
    }
</style>
