<script>
import { prevent_default } from "svelte/internal";

let search = '';
const API_URL = 'https://api.giphy.com/v1/gifs/search?api_key=JnxTmEGKXjZeUKBzRjTQoMDg8OX8pS5U&rating=pg&q=';
let gifs = [];

async function submittedRequest(event) {
	event.preventDefault();
	gifs = [];
	const url = `${API_URL}${search}`;
	const response = await fetch(url);
	const json = await response.json();
	gifs = json.data.map(gif => gif.images.fixed_height.url);
}
</script>

<style>
.results {
	column-count: 3;
	margin-top: 30px;
}

img {
	width: 100%;
	height: auto;
}

form {
	text-align: center;
	position: relative;
	left: 50%;
	transform: translateX(-50%);
}

#submit {
	background-color: #0f62fe;
	color: #f4f4f4;
}
</style>

<form on:submit={submittedRequest}>
	<h1>Let`s search some gifs</h1>
	<input bind:value={search} id="search" name="search" type="text" />
	<button id="submit" type="submit">GO</button>
</form>

<div class="results">
	{#each gifs as gif}
		<img src="{gif}" alt="gif">
	{/each}
</div>
