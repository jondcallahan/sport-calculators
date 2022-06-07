<script lang="ts">
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
  <h1>Welcome to walk run pace calculator</h1>
  <p>
    This calculator will tell you your average pace for a given walk / run timer
    and walking / running pace.
  </p>

  <!-- Walk / run split -->

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
</main>

<style>
  :root {
    font-family: "Helvetica Neue", -apple-system, BlinkMacSystemFont, "Segoe UI",
      Roboto, Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue",
      sans-serif;
  }

  main {
    max-width: 72ch;
    margin: 0 auto;
  }

  .grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-gap: 1rem;
  }
</style>
