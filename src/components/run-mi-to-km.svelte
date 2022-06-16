<script>
  let milesInputMinutes = 0;
  let milesInputSeconds = 0;

  let kmInputMinutes = 0;
  let kmInputSeconds = 0;

  $: milesPace = milesInputMinutes * 60 + milesInputSeconds; // In seconds
  $: kmPace = kmInputMinutes * 60 + kmInputSeconds; // In seconds

  $: milesPaceInKm = Math.round(milesPace / 1.609344);
  $: milesPaceInKmMins = Math.floor(milesPaceInKm / 60);
  $: milesPaceInKmSeconds = (milesPaceInKm % 60).toString().padStart(2, "0");

  $: kmPaceInMiles = Math.round(kmPace * 1.609344);
  $: kmPaceInMilesMins = Math.floor(kmPaceInMiles / 60);
  $: kmPaceInMilesSeconds = (kmPaceInMiles % 60).toString().padStart(2, "0");
</script>

<section>
  <h2>Convert run pace between miles and kilometers.</h2>
  <section>
    <p>Convert minutes per mile to minutes per kilometer.</p>
    <p>Pace in minutes per mile:</p>
    <label for="paceInMilesMins">
      <span>Minutes</span>
    </label>
    <input
      type="number"
      name="paceInMilesMins"
      id="paceInMilesMins"
      bind:value={milesInputMinutes}
      min="0"
    />

    <label for="paceInMilesSeconds">
      <span>Seconds</span>
    </label>
    <input
      type="number"
      name="paceInMilesSeconds"
      id="paceInMilesSeconds"
      bind:value={milesInputSeconds}
      min="0"
    />

    <p>
      Your pace is <strong
        >{milesPaceInKmMins}:{milesPaceInKmSeconds} / km</strong
      >
    </p>
  </section>

  <br />

  <section>
    <p>Convert pace in minutes per kilometer to minutes per mile.</p>
    <p>Pace in minutes per kilometer:</p>
    <label for="paceInKmMins">
      <span>Minutes</span>
    </label>
    <input
      type="number"
      name="paceInKmMins"
      id="paceInKmMins"
      bind:value={kmInputMinutes}
      min="0"
    />

    <label for="paceInKmSeconds">
      <span>Seconds</span>
    </label>
    <input
      type="number"
      name="paceInKmSeconds"
      id="paceInKmSeconds"
      bind:value={kmInputSeconds}
      min="0"
    />

    <p>
      Your pace is <strong
        >{kmPaceInMilesMins}:{kmPaceInMilesSeconds} / mi</strong
      >
    </p>
  </section>

  <section />
</section>
