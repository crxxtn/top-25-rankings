<script>
	import { bind, each } from 'svelte/internal';
	import { onMount } from 'svelte';

	function getData() {
		fetch('http://site.api.espn.com/apis/site/v2/sports/football/college-football/rankings')
			.then((response) => {
				if (response.ok) return response.json();
				return Promise.reject(response);
			})
			.then(function (data) {
				if (pollType === 'ap') {
					rankings = data.rankings[0].ranks;
				} else {
					rankings = data.rankings[1].ranks;
				}
			})
			.catch(function (err) {
				console.warn(err);
			});
	}

	onMount(getData);

	let rankings = [];

	let pollType = 'ap';
</script>

<select bind:value={pollType} on:change={getData}>
	<option value="ap">AP</option>
	<option value="coaches">Coaches</option>
</select>

<table class="mx-auto sm:text-xl text-white bg-gray-900">
	{#each rankings as ranking}
		<tr class="odd:bg-gray-800">
			<td class="font-bold text-3xl">{ranking.current.toString().padStart(2, '\xa0')}</td>
			<td>
				<img class="inline w-8 mr-2" src={ranking.team.logo} alt={ranking.team.name} />
				<span class="tracking-wider">{ranking.team.nickname}</span>
			</td>
			<td>
				{ranking.points} ({ranking.firstPlaceVotes})
			</td>
			<td
				class={ranking.trend[0] === '+'
					? 'text-green-600'
					: ranking.trend === '-'
					? 'text-black'
					: 'text-red-600'}
			>
				{ranking.trend}
			</td>
		</tr>
	{/each}
</table>

<style>
	td {
		@apply p-4;
	}
</style>
