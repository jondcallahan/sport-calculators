<script>
  import { onMount } from "svelte";

  let milesInputMinutes = 0;
  let milesInputSeconds = 0;

  let kmInputMinutes = 0;
  let kmInputSeconds = 0;

  // Flag to prevent saving before loading is complete
  let isLoaded = false;

  $: milesPace = milesInputMinutes * 60 + milesInputSeconds; // In seconds
  $: kmPace = kmInputMinutes * 60 + kmInputSeconds; // In seconds

  $: milesPaceInKm = Math.round(milesPace / 1.609344);
  $: milesPaceInKmMins = Math.floor(milesPaceInKm / 60);
  $: milesPaceInKmSeconds = (milesPaceInKm % 60).toString().padStart(2, "0");

  $: kmPaceInMiles = Math.round(kmPace * 1.609344);
  $: kmPaceInMilesMins = Math.floor(kmPaceInMiles / 60);
  $: kmPaceInMilesSeconds = (kmPaceInMiles % 60).toString().padStart(2, "0");

  // Load saved values from localStorage on mount
  onMount(() => {
    if (typeof localStorage !== "undefined") {
      const savedMilesMinutes = localStorage.getItem("run-milesMinutes");
      const savedMilesSeconds = localStorage.getItem("run-milesSeconds");
      const savedKmMinutes = localStorage.getItem("run-kmMinutes");
      const savedKmSeconds = localStorage.getItem("run-kmSeconds");

      if (savedMilesMinutes) milesInputMinutes = parseFloat(savedMilesMinutes);
      if (savedMilesSeconds) milesInputSeconds = parseFloat(savedMilesSeconds);
      if (savedKmMinutes) kmInputMinutes = parseFloat(savedKmMinutes);
      if (savedKmSeconds) kmInputSeconds = parseFloat(savedKmSeconds);
    }

    // Mark as loaded to enable saving
    isLoaded = true;
  });

  // Save values to localStorage when they change (only after initial load)
  $: if (isLoaded && typeof localStorage !== "undefined") {
    localStorage.setItem("run-milesMinutes", milesInputMinutes.toString());
    localStorage.setItem("run-milesSeconds", milesInputSeconds.toString());
    localStorage.setItem("run-kmMinutes", kmInputMinutes.toString());
    localStorage.setItem("run-kmSeconds", kmInputSeconds.toString());
  }
</script>

<section class="calculator-card">
  <h2>🏃‍♂️ Running Pace Calculator</h2>
  <p class="card-description">
    Convert running pace between miles and kilometers. Perfect for international
    races or when following training plans in different units.
  </p>

  <div class="converter-section">
    <h3>Miles → Kilometers</h3>
    <p class="converter-description">
      Convert your pace per <strong>mile</strong> to equivalent pace per
      <strong>kilometer</strong>
    </p>

    <div class="time-input-group">
      <span class="time-label">Pace per mile</span>
      <div class="time-inputs">
        <div class="input-group">
          <label for="paceInMilesMins">Minutes</label>
          <input
            type="number"
            name="paceInMilesMins"
            id="paceInMilesMins"
            bind:value={milesInputMinutes}
            min="0"
            max="59"
            inputmode="numeric"
            class="number-input"
          />
        </div>
        <span class="time-separator">:</span>
        <div class="input-group">
          <label for="paceInMilesSeconds">Seconds</label>
          <input
            type="number"
            name="paceInMilesSeconds"
            id="paceInMilesSeconds"
            bind:value={milesInputSeconds}
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
        {milesPaceInKmMins}:{milesPaceInKmSeconds} per km
      </span>
    </div>
  </div>

  <div class="converter-section">
    <h3>Kilometers → Miles</h3>
    <p class="converter-description">
      Convert your pace per <strong>kilometer</strong> to equivalent pace per
      <strong>mile</strong>
    </p>

    <div class="time-input-group">
      <span class="time-label">Pace per kilometer</span>
      <div class="time-inputs">
        <div class="input-group">
          <label for="paceInKmMins">Minutes</label>
          <input
            type="number"
            name="paceInKmMins"
            id="paceInKmMins"
            bind:value={kmInputMinutes}
            min="0"
            max="59"
            inputmode="numeric"
            class="number-input"
          />
        </div>
        <span class="time-separator">:</span>
        <div class="input-group">
          <label for="paceInKmSeconds">Seconds</label>
          <input
            type="number"
            name="paceInKmSeconds"
            id="paceInKmSeconds"
            bind:value={kmInputSeconds}
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
        {kmPaceInMilesMins}:{kmPaceInMilesSeconds} per mile
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
    background: linear-gradient(135deg, #059669, #047857);
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
