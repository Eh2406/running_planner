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
    value={speed}
    min="1"
    max="7"
    step="0.1"
    on:input={event => {
      speed = +event.target.value;
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
    value={time}
    min="1"
    max="80"
    step="0.25"
    on:input={event => {
      time = +event.target.value;
      if (pined) {
        speed = +(pinedDist / time).toFixed(2);
      }
    }} />
  min
</div>
<div>
  <input
    type="checkbox"
    checked={pined}
    on:input={function() {
      pined = this.checked;
      pinedDist = speed * time;
    }} />
  {dist.toFixed(2)} mile
</div>
