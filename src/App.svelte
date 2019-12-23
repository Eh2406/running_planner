<script>
  let reps = 3;
  let utime = 5;
  let uspeed = 3.2;
  let wtime = 1;
  let wspeed = 3.2;
  let rtime = 17;
  let rspeed = 4.4;

  $: ttime = 2 * utime + reps * (wtime + rtime) - wtime;
  $: tdist =
    2 * ((utime / 60) * uspeed) +
    reps * ((wtime / 60) * wspeed + (rtime / 60) * rspeed) -
    (wtime / 60) * wspeed;
</script>

<style>
  .warmup {
    background-color: brown;
  }
  .run {
    background-color: red;
  }
  .walk {
    background-color: yellow;
  }
  .row-cart {
    display: flex;
  }
  .row-cart > div {
    display: inline-block;
  }
  .form {
    display: inline-block;
  }
  .report {
    display: inline-block;
  }
  .all {
    display: flex;
  	justify-content: space-evenly;
  }
</style>

<h1>Let's plan a run! :)</h1>
<div class="all">
  <div class="form">
    <label>Reps:</label>
    <input
      type="number"
      bind:value={reps}
      name="reps"
      min="1"
      max="10"
      step="1" />

    <label>Warmup-time:</label>
    <input
      type="number"
      bind:value={utime}
      name="utime"
      min="1"
      max="10"
      step="0.25" />
    min
    <label>Warmup-speed:</label>
    <input
      type="number"
      bind:value={uspeed}
      name="uspeed"
      min="1"
      max="7"
      step="0.1" />
    mph
    <label>Walk-time:</label>
    <input
      type="number"
      bind:value={wtime}
      name="wtime"
      min="1"
      max="10"
      step="0.25" />
    min
    <label>Walk-speed:</label>
    <input
      type="number"
      bind:value={wspeed}
      name="wspeed"
      min="1"
      max="7"
      step="0.1" />
    mph
    <label>Run-time:</label>
    <input
      type="number"
      bind:value={rtime}
      name="rtime"
      min="1"
      max="10"
      step="0.25" />
    min
    <label>Run-speed:</label>
    <input
      type="number"
      bind:value={rspeed}
      name="rspeed"
      min="1"
      max="7"
      step="0.1" />
    mph
  </div>
  <div class="report">
    <h2>This will take {ttime} min</h2>
    <div class="row-cart">
      <div class="warmup" style="width: {100 * (utime / ttime)}%;">&nbsp;</div>
      {#each new Array(reps) as _, i}
        <div class="run" style="width: {100 * (rtime / ttime)}%;">&nbsp;</div>
        {#if i + 1 !== reps}
          <div class="walk" style="width: {100 * (wtime / ttime)}%;">
            &nbsp;
          </div>
        {/if}
      {/each}
      <div class="warmup" style="width: {100 * (utime / ttime)}%;">&nbsp;</div>
    </div>
    <h2>
      and will go {tdist.toFixed(2)} miles / {(tdist * 1.60934).toFixed(2)} km
    </h2>
    <div class="row-cart">
      <div
        class="warmup"
        style="width: {100 * (((utime / 60) * uspeed) / tdist)}%;">
        &nbsp;
      </div>
      {#each new Array(reps) as _, i}
        <div
          class="run"
          style="width: {100 * (((rtime / 60) * rspeed) / tdist)}%;">
          &nbsp;
        </div>
        {#if i + 1 !== reps}
          <div
            class="walk"
            style="width: {100 * (((wtime / 60) * wspeed) / tdist)}%;">
            &nbsp;
          </div>
        {/if}
      {/each}
      <div
        class="warmup"
        style="width: {100 * (((utime / 60) * uspeed) / tdist)}%;">
        &nbsp;
      </div>
    </div>
    <ol>
      <li>Warmup: {utime} @ {uspeed} mph</li>
      {#each new Array(reps) as _, i}
        <li>Run: {rtime} @ {rspeed} mph</li>
        {#if i + 1 !== reps}
          <li>Walk: {wtime} @ {wspeed} mph</li>
        {/if}
      {/each}
      <li>Cooldown: {utime} @ {uspeed} mph</li>
    </ol>
  </div>

</div>
