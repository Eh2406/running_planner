<script>
  export let preaf;
  export let speed;
  export let time;
  export let dist;
  let pined = false;
  let pinedDist = 1;
</script>

<div style="grid-column: 1;">
  <label>{preaf}-speed:</label>
  <input
    type="number"
    bind:value={speed}
    min="1"
    max="7"
    step="0.1"
    on:change={() => {
      if (pined) {
        time = +(pinedDist / speed).toFixed(2);
      }
    }} />
  mph
</div>
<div>
  <label>{preaf}-time:</label>
  <input
    type="number"
    bind:value={time}
    min="1"
    max="80"
    step="0.25"
    on:change={() => {
      if (pined) {
        speed = +(pinedDist / time).toFixed(2);
      }
    }} />
  min
</div>
<div>
  <input
    type="checkbox"
    bind:checked={pined}
    on:change={() => (pinedDist = speed * time)} />
  {dist.toFixed(2)} mile
</div>
