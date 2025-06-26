<script>
  import { onMount } from "svelte";

  let yardsInputMinutes = 0;
  let yardsInputSeconds = 0;

  let metersInputMinutes = 0;
  let metersInputSeconds = 0;

  // Flag to prevent saving before loading is complete
  let isLoaded = false;

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

  // Load saved values from localStorage on mount
  onMount(() => {
    if (typeof localStorage !== "undefined") {
      const savedYardsMinutes = localStorage.getItem("swim-yardsMinutes");
      const savedYardsSeconds = localStorage.getItem("swim-yardsSeconds");
      const savedMetersMinutes = localStorage.getItem("swim-metersMinutes");
      const savedMetersSeconds = localStorage.getItem("swim-metersSeconds");

      if (savedYardsMinutes) yardsInputMinutes = parseFloat(savedYardsMinutes);
      if (savedYardsSeconds) yardsInputSeconds = parseFloat(savedYardsSeconds);
      if (savedMetersMinutes)
        metersInputMinutes = parseFloat(savedMetersMinutes);
      if (savedMetersSeconds)
        metersInputSeconds = parseFloat(savedMetersSeconds);
    }

    // Mark as loaded to enable saving
    isLoaded = true;
  });

  // Save values to localStorage when they change (only after initial load)
  $: if (isLoaded && typeof localStorage !== "undefined") {
    localStorage.setItem("swim-yardsMinutes", yardsInputMinutes.toString());
    localStorage.setItem("swim-yardsSeconds", yardsInputSeconds.toString());
    localStorage.setItem("swim-metersMinutes", metersInputMinutes.toString());
    localStorage.setItem("swim-metersSeconds", metersInputSeconds.toString());
  }
</script>

<section class="calculator-card">
  <h2>🏊‍♂️ Swimming Pace Calculator</h2>
  <p class="card-description">
    Convert swimming pace between yards and meters. Useful when training in
    different pools or comparing times from meets in different units.
  </p>

  <div class="converter-section">
    <h3>Yards → Meters</h3>
    <p class="converter-description">
      Convert your pace per 100 <strong>yards</strong> to equivalent pace per
      100 <strong>meters</strong>
    </p>

    <div class="time-input-group">
      <span class="time-label">Pace per 100 yards</span>
      <div class="time-inputs">
        <div class="input-group">
          <label for="paceInYardsMinutes">Minutes</label>
          <input
            type="number"
            name="paceInYardsMinutes"
            id="paceInYardsMinutes"
            bind:value={yardsInputMinutes}
            min="0"
            max="59"
            inputmode="numeric"
            class="number-input"
          />
        </div>
        <span class="time-separator">:</span>
        <div class="input-group">
          <label for="paceInYardsSeconds">Seconds</label>
          <input
            type="number"
            name="paceInYardsSeconds"
            id="paceInYardsSeconds"
            bind:value={yardsInputSeconds}
            min="0"
            max="59"
            inputmode="numeric"
            class="number-input"
          />
        </div>
      </div>
    </div>

    <div class="result-card">
      <span class="result-label">Equivalent Pace</span>
      <span class="result-value">
        {yardsPaceInMetersMinutes}:{yardsPaceInMetersSeconds} per 100m
      </span>
    </div>
  </div>

  <div class="converter-section">
    <h3>Meters → Yards</h3>
    <p class="converter-description">
      Convert your pace per 100 <strong>meters</strong> to equivalent pace per
      100 <strong>yards</strong>
    </p>

    <div class="time-input-group">
      <span class="time-label">Pace per 100 meters</span>
      <div class="time-inputs">
        <div class="input-group">
          <label for="paceInMetersMinutes">Minutes</label>
          <input
            type="number"
            name="paceInMetersMinutes"
            id="paceInMetersMinutes"
            bind:value={metersInputMinutes}
            min="0"
            max="59"
            inputmode="numeric"
            class="number-input"
          />
        </div>
        <span class="time-separator">:</span>
        <div class="input-group">
          <label for="paceInMetersSeconds">Seconds</label>
          <input
            type="number"
            name="paceInMetersSeconds"
            id="paceInMetersSeconds"
            bind:value={metersInputSeconds}
            min="0"
            max="59"
            inputmode="numeric"
            class="number-input"
          />
        </div>
      </div>
    </div>

    <div class="result-card">
      <span class="result-label">Equivalent Pace</span>
      <span class="result-value">
        {metersPaceInYardsMinutes}:{metersPaceInYardsSeconds} per 100yd
      </span>
    </div>
  </div>
</section>

<style>
  .calculator-card {
    background: var(--card-background, #ffffff);
    border-radius: var(--border-radius, 12px);
    padding: var(--spacing-lg, 2rem);
    box-shadow: var(--shadow-md, 0 4px 6px -1px rgb(0 0 0 / 0.1));
    border: 1px solid var(--border-color, #e5e7eb);
    margin-bottom: var(--spacing-lg, 2rem);
  }

  .calculator-card h2 {
    font-size: 1.5rem;
    font-weight: 600;
    margin: 0 0 var(--spacing-xs, 0.5rem) 0;
    color: var(--text-primary, #1a1a1a);
  }

  .card-description {
    color: var(--text-secondary, #6b7280);
    margin: 0 0 var(--spacing-lg, 2rem) 0;
    font-size: 1rem;
  }

  .converter-section {
    margin-bottom: var(--spacing-xl, 3rem);
  }

  .converter-section:last-child {
    margin-bottom: 0;
  }

  .converter-section h3 {
    font-size: 1.125rem;
    font-weight: 500;
    margin: 0 0 var(--spacing-xs, 0.5rem) 0;
    color: var(--text-primary, #1a1a1a);
  }

  .converter-description {
    color: var(--text-secondary, #6b7280);
    margin: 0 0 var(--spacing-md, 1.5rem) 0;
    font-size: 0.875rem;
  }

  .time-input-group {
    margin-bottom: var(--spacing-md, 1.5rem);
  }

  .time-label {
    display: block;
    font-size: 0.875rem;
    font-weight: 500;
    color: var(--text-primary, #1a1a1a);
    margin-bottom: var(--spacing-xs, 0.5rem);
  }

  .time-inputs {
    display: flex;
    align-items: flex-end;
    gap: var(--spacing-sm, 1rem);
    max-width: 300px;
  }

  .input-group {
    display: flex;
    flex-direction: column;
    gap: var(--spacing-xs, 0.5rem);
    flex: 1;
  }

  .input-group label {
    font-size: 0.75rem;
    font-weight: 500;
    color: var(--text-secondary, #6b7280);
    text-align: center;
  }

  .time-separator {
    font-size: 1.5rem;
    font-weight: 600;
    color: var(--text-secondary, #6b7280);
    margin-bottom: 0.5rem;
  }

  .number-input {
    padding: 0.75rem 1rem;
    border: 2px solid var(--border-color, #e5e7eb);
    border-radius: 8px;
    font-size: 1rem;
    background: var(--card-background, #ffffff);
    transition: all 0.2s ease;
    text-align: center;
    width: 100%;
  }

  .number-input:focus {
    outline: none;
    border-color: var(--primary-color, #0066cc);
    box-shadow: 0 0 0 3px var(--primary-light, #e6f3ff);
  }

  .number-input:hover {
    border-color: #9ca3af;
  }

  .result-card {
    background: linear-gradient(135deg, #0891b2, #0e7490);
    color: white;
    padding: var(--spacing-md, 1.5rem);
    border-radius: var(--border-radius, 12px);
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: var(--spacing-xs, 0.5rem);
    box-shadow: var(--shadow-lg, 0 10px 15px -3px rgb(0 0 0 / 0.1));
  }

  .result-label {
    font-size: 0.875rem;
    font-weight: 500;
    opacity: 0.9;
  }

  .result-value {
    font-size: 1.5rem;
    font-weight: 700;
    font-variant-numeric: tabular-nums;
  }

  /* Responsive design */
  @media (max-width: 640px) {
    .calculator-card {
      padding: var(--spacing-md, 1.5rem);
    }

    .time-inputs {
      max-width: none;
    }
  }
</style>
