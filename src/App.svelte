<script lang="ts">
  import Swim from "./components/swim.svelte";
  let runTime = 5;
  let walkTime = 1;
  let runPace = 9; // minutes per mile
  let walkPace = 16; // minutes per mile

  $: runDistance = runTime / runPace;
  $: walkDistance = walkTime / walkPace;
  $: totalDistance = runDistance + walkDistance;
  $: totalTime = runTime + walkTime;

  $: combined = totalTime / totalDistance;
  // combined is a float, convert it to minutes and seconds
  $: combinedMinutes = Math.floor(combined);
  $: combinedSeconds = Math.round(
    Math.floor((combined - combinedMinutes) * 60)
  );
</script>

<main>
  <section>
    <h1>Welcome to Jon&rsquo;s sports calculators</h1>
    <p>
      This is a collection of calculators that I use for swim, bike, and run
      training.
    </p>
  </section>
  <hr />

  <!-- Walk / run split -->

  <h2>Get average pace duing a walk-run.</h2>
  <section class="grid">
    <label for="runTime">
      <span>Run time in minutes</span>
    </label>
    <input
      type="number"
      name="runTime"
      id="runTime"
      bind:value={runTime}
      step="0.5"
    />

    <label for="walkTime">
      <span>Walk time in minutes</span>
    </label>
    <input
      type="number"
      name="walkTime"
      id="walkTime"
      bind:value={walkTime}
      step="0.5"
    />
  </section>

  <br />

  <section class="grid">
    <label for="runPace">
      <span>Run pace in minutes per mile</span>
    </label>
    <input
      type="number"
      name="runPace"
      id="runPace"
      bind:value={runPace}
      step="0.25"
    />

    <label for="walkPace">
      <span>Walk pace in minutes per mile</span>
    </label>
    <input
      type="number"
      name="walkPace"
      id="walkPace"
      bind:value={walkPace}
      step="0.25"
    />
  </section>

  <br />

  <!-- Now combine the runTime and runPace with walkTime and walkPace to get combined average pace -->
  <p>
    Your average pace is <strong
      >{combinedMinutes} minutes {combinedSeconds} seconds</strong
    > per mile.
  </p>

  <br />
  <hr />
  <!-- Swim split -->
  <Swim />
</main>

<style>
  :root {
    font-family: Inter, sans-serif;
    background-color: rgb(245, 245, 247);
  }

  main {
    max-width: 72ch;
    margin: 1rem auto;
    background-color: white;
    border-radius: 18px;
    padding: 1rem;
    box-shadow: 0 0.5rem 1rem rgba(0, 0, 0, 0.15);
  }

  .grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-gap: 1rem;
  }

  input {
    padding: 0.5rem;
    border: 1px solid #ccc;
    border-radius: 4px;
    box-shadow: rgb(255, 255, 255) 0px 0px 0px 0px,
      rgba(0, 0, 0, 0.05) 0px 0px 0px 1px,
      rgba(0, 0, 0, 0.05) 0px 2px 4px 0px inset;
  }
</style>
