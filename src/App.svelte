<script>
  import SpeedTime from "./SpeedTime.svelte";

  let reps = 3;
  let utime = 5;
  let uspeed = 3.2;
  let wtime = 1;
  let wspeed = 3.2;
  let rtime = 17;
  let rspeed = 4.4;

  $: udist = (utime / 60) * uspeed;
  $: rdist = (rtime / 60) * rspeed;
  $: wdist = (wtime / 60) * wspeed;

  $: ttime = 2 * utime + reps * (wtime + rtime) - wtime;
  $: tdist = 2 * udist + reps * (wdist + rdist) - wdist;

  $: arrayOfIntervals = (() => {
    let timeSoFar = 0;
    let distSoFar = 0;
    let out = [];
    timeSoFar += utime;
    distSoFar += udist;
    out.push({
      type: "warmup",
      time: utime,
      dist: udist,
      speed: uspeed,
      timeSoFar,
      distSoFar
    });

    for (let i = 0; i < reps; i += 1) {
      timeSoFar += rtime;
      distSoFar += rdist;
      out.push({
        type: "run",
        time: rtime,
        dist: rdist,
        speed: rspeed,
        timeSoFar,
        distSoFar
      });
      if (i + 1 !== reps) {
        timeSoFar += wtime;
        distSoFar += wdist;
        out.push({
          type: "walk",
          time: wtime,
          dist: wdist,
          speed: wspeed,
          timeSoFar,
          distSoFar
        });
      }
    }

    timeSoFar += utime;
    distSoFar += udist;
    out.push({
      type: "warmup",
      time: utime,
      dist: udist,
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
    <SpeedTime
      preaf="Warmup"
      bind:speed={uspeed}
      bind:time={utime}
      bind:dist={udist} />
    <SpeedTime
      preaf="Walk"
      bind:speed={wspeed}
      bind:time={wtime}
      bind:dist={wdist} />
    <SpeedTime
      preaf="Run"
      bind:speed={rspeed}
      bind:time={rtime}
      bind:dist={rdist} />
  </div>
  <div class="report">
    <h2>This will take {ttime.toFixed(2)} min</h2>
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
    <table>
      {#each arrayOfIntervals as interval, i}
        <tr class={interval.type}>
          <th>{i + 1}</th>
          <td>{{ warmup: '🧘', run: '🏃', walk: '🚶' }[interval.type]}</td>
          <td>{interval.time} @ {interval.speed} mph</td>
          <td>
            Stoping at {interval.distSoFar.toFixed(2)} mile / {(interval.distSoFar * 1.60934).toFixed(2)}
            km
          </td>
        </tr>
      {/each}
    </table>
  </div>

</div>
