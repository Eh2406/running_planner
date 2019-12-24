<script>
  import Runs from "./Runs.svelte";

  let runs = [];

  function addARun() {
    runs.push({
      reps: 3,
      utime: 5,
      uspeed: 3.2,
      wtime: 1,
      wspeed: 3.2,
      rtime: 17,
      rspeed: 4.4
    });
    runs = runs;
  }
  addARun();
</script>

<style>
  button {
    margin: 10px;
  }
</style>

<h1>Let's plan a run! :)</h1>
<ol>
  {#each runs as run}
    <li>
      <Runs {...runs} />
    </li>
  {/each}
</ol>
<button on:click={addARun}>+</button>
