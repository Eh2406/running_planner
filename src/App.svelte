<script>
  import Runs from "./Runs.svelte";

  let runs = JSON.parse(localStorage.getItem("runs")) || [];

  $: localStorage.setItem("runs", JSON.stringify(runs));

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
  if (runs.length === 0) {
    addARun();
  }
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
<button
  on:click={() => {
    runs = [];
  }}>
  Clere All
</button>
