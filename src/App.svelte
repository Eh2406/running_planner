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
  if (runs.length === 0) {
    maxID = 0;
    addARun();
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
  let:index>
  <Runs
    bind:reps={runs[index].reps}
    bind:utime={runs[index].utime}
    bind:uspeed={runs[index].uspeed}
    bind:wtime={runs[index].wtime}
    bind:wspeed={runs[index].wspeed}
    bind:rtime={runs[index].rtime}
    bind:rspeed={runs[index].rspeed}
    on:dell={() => {
      runs.splice(index, 1);
      runs = runs;
    }}
    on:duplicate={() => {
      maxID += 1;
      let new_run = Object.assign({}, runs[index]);
      new_run.id = maxID;
      runs.splice(index + 1, 0, new_run);
      runs = runs;
    }} />
</SortableList>
<button on:click={addARun}>+</button>
<button
  on:click={() => {
    maxID = 0;
    runs = [];
  }}>
  Clear All
</button>
