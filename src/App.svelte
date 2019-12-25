<script>
  import Runs from "./Runs.svelte";
  import SortableList from "svelte-sortable-list";

  let runs = JSON.parse(localStorage.getItem("runs")) || [];

  let maxID = Math.max(...runs.map(r => r.id)) || 0;

  $: localStorage.setItem("runs", JSON.stringify(runs));

  function addARun() {
    maxID += 1;
    runs.push({
      id: maxID,
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
</script>

<style>
  button {
    margin: 10px;
  }
</style>

<h1>Let's plan a run! :)</h1>
<SortableList
  bind:list={runs}
  key="id"
  on:sort={ev => {
    runs = ev.detail;
  }}
  let:item>
  <Runs {...item} />
</SortableList>
<button on:click={addARun}>+</button>
<button
  on:click={() => {
    maxID = 0;
    runs = [];
  }}>
  Clear All
</button>
