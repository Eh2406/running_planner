<script>
  export let preaf;
  export let speed;
  export let time;
  export let dist;
</script>

<div style="grid-column: 1;">
  <label>{preaf}-speed:</label>
  <input type="number" bind:value={speed} min="1" max="7" step="0.1" />
  mph
</div>
<div>
  <label>{preaf}-time:</label>
  <input type="number" bind:value={time} min="1" max="10" step="0.25" />
  min
</div>
<div>{dist.toFixed(2)} mile</div>
