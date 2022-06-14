<script>
  let yardsInputMinutes = 0;
  let yardsInputSeconds = 0;

  let metersInputMinutes = 0;
  let metersInputSeconds = 0;

  $: yardsPace = yardsInputMinutes * 60 + yardsInputSeconds; // In seconds
  $: metersPace = metersInputMinutes * 60 + metersInputSeconds; // In seconds

  // convert the combined pace in yards to a combined pace in meters
  $: yardsPaceInMeters = Math.round(yardsPace / 0.9144);
  // Turn yardsPaceInMeters into a pace in minutes and seconds
  $: yardsPaceInMetersMinutes = Math.floor(yardsPaceInMeters / 60);
  $: yardsPaceInMetersSeconds = (yardsPaceInMeters % 60)
    .toString()
    .padStart(2, "0");

  // convert the combined pace in meters to a combined pace in yards
  $: metersPaceInYards = Math.round(metersPace * 0.9144);
  // Turn metersPaceInYards into a pace in minutes and seconds
  $: metersPaceInYardsMinutes = Math.floor(metersPaceInYards / 60);
  $: metersPaceInYardsSeconds = (metersPaceInYards % 60)
    .toString()
    .padStart(2, "0");
</script>

<section>
  <h2>Convert swim pace between yards and meters.</h2>
  <section>
    <p>
      Convert pace per 100 <strong>yards</strong> to pace per 100
      <strong>meters</strong>.
    </p>
    <p>Pace per 100 yards</p>
    <label for="paceInYardsMinutes">
      <span>Minutes</span>
    </label>
    <input
      type="number"
      name="paceInYardsMinutes"
      id="paceInYardsMinutes"
      bind:value={yardsInputMinutes}
      min="0"
    />

    <label for="paceInYardsSeconds">
      <span>Seconds</span>
    </label>
    <input
      type="number"
      name="paceInYardsSeconds"
      id="paceInYardsSeconds"
      bind:value={yardsInputSeconds}
      min="0"
    />

    <p>
      Your pace is <strong
        >{yardsPaceInMetersMinutes}:{yardsPaceInMetersSeconds} / 100 meters</strong
      >
    </p>
  </section>

  <br />

  <section>
    <p>
      Convert pace per 100 <strong>meters</strong> to pace per 100
      <strong>yards</strong>.
    </p>
    <p>Pace per 100 meters</p>
    <label for="paceInMetersMinutes">
      <span>Minutes</span>
    </label>
    <input
      type="number"
      name="paceInMetersMinutes"
      id="paceInMetersMinutes"
      bind:value={metersInputMinutes}
      min="0"
    />

    <label for="paceInMetersSeconds">
      <span>Seconds</span>
    </label>
    <input
      type="number"
      name="paceInMetersSeconds"
      id="paceInMetersSeconds"
      bind:value={metersInputSeconds}
      min="0"
    />

    <p>
      Your pace is <strong
        >{metersPaceInYardsMinutes}:{metersPaceInYardsSeconds} / 100 yards</strong
      >
    </p>
  </section>

  <section />
</section>
