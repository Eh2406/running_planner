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

  $: arrayOfIntervals = (() => {
    let timeSoFar = 0;
    let distSoFar = 0;
    let out = [];
    timeSoFar += utime;
    distSoFar += (utime / 60) * uspeed;
    out.push({
      type: "warmup",
      time: utime,
      dist: (utime / 60) * uspeed,
      speed: uspeed,
      timeSoFar,
      distSoFar
    });

    for (let i = 0; i < reps; i += 1) {
      timeSoFar += rtime;
      distSoFar += (rtime / 60) * rspeed;
      out.push({
        type: "run",
        time: rtime,
        dist: (rtime / 60) * rspeed,
        speed: rspeed,
        timeSoFar,
        distSoFar
      });
      if (i + 1 !== reps) {
        timeSoFar += wtime;
        distSoFar += (wtime / 60) * wspeed;
        out.push({
          type: "walk",
          time: wtime,
          dist: (wtime / 60) * wspeed,
          speed: wspeed,
          timeSoFar,
          distSoFar
        });
      }
    }

    timeSoFar += utime;
    distSoFar += (utime / 60) * uspeed;
    out.push({
      type: "warmup",
      time: utime,
      dist: (utime / 60) * uspeed,
      speed: uspeed,
      timeSoFar,
      distSoFar
    });
    return out;
  })();
</script>

<style>
  .warmup {
    background-color: rgba(63, 121, 247, 0.5);
  }
  .run {
    background-color: rgb(30, 255, 0, 0.5);
  }
  .walk {
    background-color: rgb(255, 0, 0, 0.5);
  }
  .row-cart {
    display: flex;
  }
  .row-cart > div {
    display: inline-block;
  }
  .form {
    display: inline-grid;
    grid-template-columns: repeat(3, max-content);
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
    <div>
      <label>Reps:</label>
      <input
        type="number"
        bind:value={reps}
        name="reps"
        min="1"
        max="10"
        step="1" />
    </div>
    <div style="grid-column: 1;">
      <label>Warmup-speed:</label>
      <input
        type="number"
        bind:value={uspeed}
        name="uspeed"
        min="1"
        max="7"
        step="0.1" />
      mph
    </div>
    <div>
      <label>Warmup-time:</label>
      <input
        type="number"
        bind:value={utime}
        name="utime"
        min="1"
        max="20"
        step="0.25" />
      min
    </div>
    <div style="grid-column: 1;">
      <label>Walk-speed:</label>
      <input
        type="number"
        bind:value={wspeed}
        name="wspeed"
        min="1"
        max="7"
        step="0.1" />
      mph
    </div>
    <div>
      <label>Walk-time:</label>
      <input
        type="number"
        bind:value={wtime}
        name="wtime"
        min="1"
        max="10"
        step="0.25" />
      min
    </div>
    <div style="grid-column: 1;">
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
    <div>
      <label>Run-time:</label>
      <input
        type="number"
        bind:value={rtime}
        name="rtime"
        min="1"
        max="120"
        step="0.25" />
      min
    </div>
  </div>
  <div class="report">
    <h2>This will take {ttime} min</h2>
    <div class="row-cart">
      {#each arrayOfIntervals as interval}
        <div
          class={interval.type}
          style="width: {100 * (interval.time / ttime)}%;">
          &nbsp;
        </div>
      {/each}
    </div>
    <h2>
      and will go {tdist.toFixed(2)} miles / {(tdist * 1.60934).toFixed(2)} km
    </h2>
    <div class="row-cart">
      {#each arrayOfIntervals as interval}
        <div
          class={interval.type}
          style="width: {100 * (interval.dist / tdist)}%;">
          &nbsp;
        </div>
      {/each}
    </div>
    <ol>
      {#each arrayOfIntervals as interval}
        <li class={interval.type}>
          {{ warmup: '🧘', run: '🏃', walk: '🚶' }[interval.type]}: {interval.time}
          @ {interval.speed} mph. Stoping at {interval.distSoFar.toFixed(2)}
          mile / {(interval.distSoFar * 1.60934).toFixed(2)}. km
        </li>
      {/each}
    </ol>
  </div>

</div>
