<script>
	import { onMount } from "svelte";
	import { apiData, sessionInfo } from './store.js';

	import { writable } from "svelte/store";

	const Role = {
			GUESSER: "Guesser",
			ANSWERER: "Answerer",
		};

	export let name;
	export let role = Role.GUESSER;
	export let sessionId = '';
	export let playerId = writable(localStorage.getItem('playerId') || '')

	function newSession() {
			fetch("http://localhost:4567/new")
				.then(response => response.json())
				.then(data => {
						console.log('this is the data');
						console.log(data);
						apiData.set(data);
						sessionId = data.sid;
						playerId.set(data.pid);
						localStorage.setItem('playerId', data.pid);
					}).catch(error => {
							console.log(error);
							return [];
						});
		}

	function joinSession() {
			fetch("http://localhost:4567/join/" + sessionId)
				.then(response => response.json())
				.then(data => {
						console.log('this is the data');
						console.log(data);
						apiData.set(data);
					}).catch(error => {
							console.log(error);
							return [];
						});
		}
</script>

<main>
	<h3>{name}</h3>

	{#if $playerId != ''}
	<p>player id {$playerId}</p>
	{/if}

	<button on:click={newSession}>New</button>
	<br>
	<input bind:value={sessionId}>
	<button on:click={joinSession}>Join</button>

	<!--
		<h4>Session ID: {$sessionInfo.sid}</h4>

		<p>Your Role<br>is</p>

		{#if role == Role.GUESSER}
			<h1>Guesser</h1>
		{/if}

		{#if role == Role.ANSWERER}
			<h1>Answerer</h1>
		{/if}
	-->
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
		margin-top: 0;
	}

	p {
		text-transform: uppercase;
		font-size: 0.9em;
		margin-bottom: 0;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
