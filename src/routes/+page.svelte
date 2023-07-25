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
        <th>Rank</th>
        <th>Team</th>
        <th>Votes</th>
        <th>Trend</th>
        <th>Record</th>
    </tr>
    {#each rankings as ranking}
        <p>Current {ranking.current}</p>
        <p>Previous {ranking.previous}</p>
        <p>Points {ranking.points}</p>
        <p>First Place Votes {ranking.firstPlaceVotes}</p>
        <p>Team {ranking.team.nickname} {ranking.team.name}</p>
        <p>Logo {ranking.team.logo}</p>
    {/each}
</table>