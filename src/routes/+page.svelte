<script>
	import { each } from "svelte/internal";

    fetch('http://site.api.espn.com/apis/site/v2/sports/football/college-football/rankings')
  .then((response) => {
    if (response.ok) return response.json();
    return Promise.reject(response);
  })
  .then(function (data) {
    rankings = data.rankings[0].ranks;
  })
  .catch(function (err) {
    console.warn(err);
  });

  let rankings = [];
</script>

<table>
    <tr>
        <th>Current</th>
        <th>Previous</th>
        <th>Team</th>
        <th>Points</th>
        <th>Ranking Difference</th>
    </tr>
    {#each rankings as ranking}
      <tr>
      <td>{ranking.current}</td>
      <td>{ranking.previous}</td>
      <td>
        <img class="inline-block w-8 h-8" src={ranking.team.logo} alt={ranking.team.name} />
        {ranking.team.nickname} {ranking.team.name}
      </td>
      <td>
        {ranking.points} ({ranking.firstPlaceVotes})
      </td>
      <td>
        {ranking.previous - ranking.current}
      </td>
      </tr>
    {/each}
</table>