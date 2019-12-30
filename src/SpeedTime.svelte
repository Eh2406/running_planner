<script>
  export let preaf;
  export let speed;
  export let time;
  export let dist;
  export let willChange = "d";
  export let pined = false;
  function fixUp() {
    if (willChange === "d") {
      dist = (time / 60) * speed;
    } else if (willChange === "s") {
      speed = (dist * 60) / time;
    } else if (willChange === "t") {
      time = (dist * 60) / speed;
    }
  }
  fixUp();
</script>

<tr style="grid-column: 1;">
  <th>{preaf}</th>
  <td>
    <input
      type="number"
      disabled={willChange === 's'}
      value={speed.toFixed(2)}
      min="1"
      max="7"
      step="0.1"
      on:input={event => {
        speed = +event.target.value;
        if (pined) {
          if (willChange === 'd') {
            time = (dist * 60) / speed;
          } else if (willChange === 't') {
            dist = (time / 60) * speed;
          }
        } else {
          fixUp();
        }
      }} />
  </td>
  <td>
    <input
      type="number"
      disabled={willChange === 't'}
      value={time.toFixed(2)}
      min="1"
      max="80"
      step="0.25"
      on:input={event => {
        time = +event.target.value;
        if (pined) {
          if (willChange === 'd') {
            speed = (dist * 60) / time;
          } else if (willChange === 's') {
            dist = (time / 60) * speed;
          }
        } else {
          fixUp();
        }
      }} />
  </td>
  <td>
    <input
      type="number"
      disabled={willChange === 'd'}
      value={dist.toFixed(2)}
      min="0"
      max="10"
      step="0.25"
      on:input={event => {
        dist = +event.target.value;
        if (pined) {
          if (willChange === 's') {
            time = (dist * 60) / speed;
          } else if (willChange === 't') {
            speed = (dist * 60) / time;
          }
        } else {
          fixUp();
        }
      }} />
  </td>
</tr>
