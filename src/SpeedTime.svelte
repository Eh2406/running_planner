<script>
  export let preaf;
  export let speed;
  export let time;
  export let dist;
  let pined = false;
  let pinedDist = 1;
</script>

<tr style="grid-column: 1;">
  <th>{preaf}</th>
  <td>
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
  </td>
  <td>
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
  </td>
  <td>
    <input
      type="checkbox"
      checked={pined}
      on:input={function() {
        pined = this.checked;
        pinedDist = speed * time;
      }} />
    {dist.toFixed(2)}
  </td>
</tr>
