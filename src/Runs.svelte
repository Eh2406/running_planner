<script>
  import SpeedTime from "./SpeedTime.svelte";
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  export let reps;
  export let utime;
  export let uspeed;
  export let wtime;
  export let wspeed;
  export let rdata;

  let willChange = "d";

  let udist = 0;
  let rdists = rdata.map(_ => 0);
  let wdist = 0;

  let resets = [];

  $: ttime =
    2 * utime +
    (reps - 1) * wtime +
    (rdata.length === 1
      ? reps * rdata[0][0]
      : rdata.reduce((t, r) => t + r[0], 0));
  $: tdist =
    2 * udist +
    (reps - 1) * wdist +
    (rdata.length === 1 ? reps * rdists[0] : rdists.reduce((t, r) => t + r, 0));

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
    if (resets[out.length - 1]) {
      timeSoFar = distSoFar = 0;
    }

    for (let i = 0; i < reps; i += 1) {
      let rtime;
      let rspeed;
      let rdist;
      if (rdata.length === 1) {
        rtime = rdata[0][0];
        rspeed = rdata[0][1];
        rdist = rdists[0];
      } else {
        rtime = rdata[i][0];
        rspeed = rdata[i][1];
        rdist = rdists[i];
      }
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
      if (resets[out.length - 1]) {
        timeSoFar = distSoFar = 0;
      }
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
        if (resets[out.length - 1]) {
          timeSoFar = distSoFar = 0;
        }
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
    if (resets[out.length - 1]) {
      timeSoFar = distSoFar = 0;
    }
    return out;
  })();
</script>

<style>
  .warmup {
    background-color: rgba(63, 121, 247, 0.75);
  }
  .run {
    background-color: rgb(30, 255, 0, 0.75);
  }
  .walk {
    background-color: rgb(255, 0, 0, 0.75);
  }
  .row-cart {
    display: flex;
  }
  .row-cart > div {
    display: inline-block;
  }
  .report {
    display: inline-block;
  }
  .all {
    position: relative;
    display: flex;
    justify-content: space-evenly;
    background-color: lightgray;
    margin: 10px;
    padding: 15px;
    border-radius: 20px;
  }
  .controls {
    position: absolute;
    top: 0;
    right: 0;
    margin: 10px;
  }
  .resets td,
  .resets th {
    border-bottom: 5px solid black;
  }
  .need-resets td,
  .need-resets th {
    border-bottom: 5px solid red;
  }
</style>

<div class="all">
  <div class="controls">
    <button
      on:click={() => {
        dispatch('duplicate');
      }}>
      duplicate
    </button>
    <button
      on:click={() => {
        dispatch('dell');
      }}>
      x
    </button>
  </div>
  <table>
    <tr>
      <th>Reps:</th>
      <td>
        <input
          type="number"
          bind:value={reps}
          name="reps"
          min="1"
          max="10"
          step="1" />
      </td>
      {#if reps > 1}
        <td>
          <button
            on:click={() => {
              if (rdata.length === 1) {
                var data = [];
                for (var i = 0; i < reps; i++) {
                  data.push(rdata[0].slice());
                }
                rdata = data;
              } else {
                rdata = [rdata[0]];
              }
              rdists = rdata.map(_ => 0);
            }}>
            {#if rdata.length !== 1}-{:else}+{/if}
          </button>
        </td>
      {/if}
    </tr>
    <tr>
      <th />
      <th
        on:click={_ => {
          willChange = 's';
        }}>
        Speed (mph)
      </th>
      <th
        on:click={_ => {
          willChange = 't';
        }}>
        Time (min)
      </th>
      <th
        on:click={_ => {
          willChange = 'd';
        }}>
        Dist (mile)
      </th>
    </tr>
    <SpeedTime
      preaf="Warmup"
      bind:speed={uspeed}
      bind:time={utime}
      bind:dist={udist}
      {willChange} />
    <SpeedTime
      preaf="Walk"
      bind:speed={wspeed}
      bind:time={wtime}
      bind:dist={wdist}
      {willChange} />
    {#each rdata as r, i}
      <SpeedTime
        preaf="Run"
        bind:speed={rdata[i][1]}
        bind:time={rdata[i][0]}
        bind:dist={rdists[i]}
        {willChange} />
    {/each}
  </table>
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
  </div>
  <table>
    {#each arrayOfIntervals as interval, i}
      <tr
        class={interval.type}
        class:resets={!!resets[i]}
        class:need-resets={interval.timeSoFar > 60}
        on:click={() => {
          resets[i] = !resets[i];
        }}>
        <th>{i + 1}</th>
        <td>{{ warmup: '🧘', run: '🏃', walk: '🚶' }[interval.type] || ''}</td>
        <td>{interval.time.toFixed(2)} @ {interval.speed.toFixed(2)} mph</td>
        <td>
          Stoping at {interval.distSoFar.toFixed(2)} mile | {interval.timeSoFar.toFixed(2)}
          min
        </td>
      </tr>
    {/each}
  </table>
</div>
