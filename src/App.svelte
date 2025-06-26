<script lang="ts">
  import { onMount } from "svelte";
  import Swim from "./components/swim.svelte";
  import RunMiToKm from "./components/run-mi-to-km.svelte";

  // Walk-run calculator state
  let runTime = 5;
  let walkTime = 1;
  let runPace = 9; // minutes per mile
  let walkPace = 16; // minutes per mile

  $: runDistance = runTime / runPace;
  $: walkDistance = walkTime / walkPace;
  $: totalDistance = runDistance + walkDistance;
  $: totalTime = runTime + walkTime;

  $: combined = totalTime / totalDistance;
  $: combinedMinutes = Math.floor(combined);
  $: combinedSeconds = Math.round((combined - combinedMinutes) * 60);

  // Tab navigation state
  let activeTab = "walk-run";

  // Flag to prevent saving before loading is complete
  let isLoaded = false;

  const tabs = [
    { id: "walk-run", label: "Walk-Run Pace", icon: "🚶‍♂️" },
    { id: "swimming", label: "Swimming Pace", icon: "🏊‍♂️" },
    { id: "running", label: "Running Pace", icon: "🏃‍♂️" },
  ];

  function setActiveTab(tabId: string) {
    activeTab = tabId;
  }

  // Load saved values from localStorage on mount
  onMount(() => {
    if (typeof localStorage !== "undefined") {
      const savedRunTime = localStorage.getItem("walkrun-runTime");
      const savedWalkTime = localStorage.getItem("walkrun-walkTime");
      const savedRunPace = localStorage.getItem("walkrun-runPace");
      const savedWalkPace = localStorage.getItem("walkrun-walkPace");
      const savedActiveTab = localStorage.getItem("activeTab");

      if (savedRunTime) runTime = parseFloat(savedRunTime);
      if (savedWalkTime) walkTime = parseFloat(savedWalkTime);
      if (savedRunPace) runPace = parseFloat(savedRunPace);
      if (savedWalkPace) walkPace = parseFloat(savedWalkPace);
      if (savedActiveTab) activeTab = savedActiveTab;
    }

    // Mark as loaded to enable saving
    isLoaded = true;
  });

  // Save values to localStorage when they change (only after initial load)
  $: if (isLoaded && typeof localStorage !== "undefined") {
    localStorage.setItem("walkrun-runTime", runTime.toString());
    localStorage.setItem("walkrun-walkTime", walkTime.toString());
    localStorage.setItem("walkrun-runPace", runPace.toString());
    localStorage.setItem("walkrun-walkPace", walkPace.toString());
    localStorage.setItem("activeTab", activeTab);
  }
</script>

<main>
  <header class="header">
    <h1>🏃‍♂️ Jon's Sports Calculators</h1>
    <p class="subtitle">Training pace calculators for runners and swimmers</p>
  </header>

  <!-- Tab Navigation -->
  <nav class="tab-nav">
    {#each tabs as tab}
      <button
        class="tab-button"
        class:active={activeTab === tab.id}
        on:click={() => setActiveTab(tab.id)}
      >
        <span class="tab-icon">{tab.icon}</span>
        <span class="tab-label">{tab.label}</span>
      </button>
    {/each}
  </nav>

  <!-- Tab Content -->
  <div class="tab-content">
    {#if activeTab === "walk-run"}
      <section class="calculator-card">
        <h2>Walk-Run Average Pace</h2>
        <p class="card-description">
          Calculate your overall pace when combining walking and running
          intervals. Perfect for run-walk training programs like Couch to 5K.
        </p>

        <div class="input-section">
          <h3>Interval Times</h3>
          <div class="input-grid">
            <div class="input-group">
              <label for="runTime">Run time (minutes)</label>
              <input
                type="number"
                name="runTime"
                id="runTime"
                bind:value={runTime}
                step="0.5"
                min="0"
                inputmode="numeric"
                class="number-input"
              />
            </div>

            <div class="input-group">
              <label for="walkTime">Walk time (minutes)</label>
              <input
                type="number"
                name="walkTime"
                id="walkTime"
                bind:value={walkTime}
                step="0.5"
                min="0"
                inputmode="numeric"
                class="number-input"
              />
            </div>
          </div>
        </div>

        <div class="input-section">
          <h3>Your Running & Walking Paces</h3>
          <div class="input-grid">
            <div class="input-group">
              <label for="runPace">Running pace (min/mile)</label>
              <input
                type="number"
                name="runPace"
                id="runPace"
                bind:value={runPace}
                step="0.25"
                min="0"
                inputmode="numeric"
                class="number-input"
              />
            </div>

            <div class="input-group">
              <label for="walkPace">Walking pace (min/mile)</label>
              <input
                type="number"
                name="walkPace"
                id="walkPace"
                bind:value={walkPace}
                step="0.25"
                min="0"
                inputmode="numeric"
                class="number-input"
              />
            </div>
          </div>
        </div>

        <div class="result-section">
          <div class="result-card">
            <span class="result-label">Your Combined Pace</span>
            <span class="result-value">
              {combinedMinutes}:{combinedSeconds.toString().padStart(2, "0")} per
              mile
            </span>
          </div>
        </div>
      </section>
    {:else if activeTab === "swimming"}
      <Swim />
    {:else if activeTab === "running"}
      <RunMiToKm />
    {/if}
  </div>
</main>

<style>
  :root {
    --primary-color: #0066cc;
    --primary-light: #e6f3ff;
    --primary-dark: #0052a3;
    --text-primary: #1a1a1a;
    --text-secondary: #6b7280;
    --background: #f8fafc;
    --card-background: #ffffff;
    --border-color: #e5e7eb;
    --border-radius: 12px;
    --shadow-sm: 0 1px 2px 0 rgb(0 0 0 / 0.05);
    --shadow-md: 0 4px 6px -1px rgb(0 0 0 / 0.1),
      0 2px 4px -2px rgb(0 0 0 / 0.1);
    --shadow-lg: 0 10px 15px -3px rgb(0 0 0 / 0.1),
      0 4px 6px -4px rgb(0 0 0 / 0.1);
    --spacing-xs: 0.5rem;
    --spacing-sm: 1rem;
    --spacing-md: 1.5rem;
    --spacing-lg: 2rem;
    --spacing-xl: 3rem;
  }

  :global(html),
  :global(body) {
    font-family:
      Inter,
      system-ui,
      -apple-system,
      sans-serif;
    background-color: var(--background);
    color: var(--text-primary);
    line-height: 1.6;
  }

  :global(*) {
    box-sizing: border-box;
  }

  main {
    max-width: 800px;
    margin: 0 auto;
    padding: var(--spacing-md);
    display: flex;
    flex-direction: column;
    gap: var(--spacing-lg);
  }

  .header {
    text-align: center;
    margin-bottom: var(--spacing-md);
  }

  .header h1 {
    font-size: 2.5rem;
    font-weight: 700;
    margin: 0 0 var(--spacing-xs) 0;
    color: var(--text-primary);
  }

  .subtitle {
    font-size: 1.125rem;
    color: var(--text-secondary);
    margin: 0;
  }

  /* Tab Navigation Styles */
  .tab-nav {
    display: flex;
    background: var(--card-background);
    border-radius: var(--border-radius);
    padding: 0.25rem;
    box-shadow: var(--shadow-sm);
    border: 1px solid var(--border-color);
    gap: 0.25rem;
    overflow-x: auto;
  }

  .tab-button {
    flex: 1;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: var(--spacing-xs);
    padding: 0.75rem 1rem;
    border: none;
    border-radius: calc(var(--border-radius) - 2px);
    background: transparent;
    color: var(--text-secondary);
    font-size: 0.875rem;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.2s ease;
    white-space: nowrap;
    min-width: fit-content;
  }

  .tab-button:hover {
    background: var(--primary-light);
    color: var(--primary-color);
  }

  .tab-button.active {
    background: var(--primary-color);
    color: white;
    box-shadow: var(--shadow-sm);
  }

  .tab-icon {
    font-size: 1rem;
  }

  .tab-label {
    font-weight: 500;
  }

  /* Tab Content */
  .tab-content {
    min-height: 400px;
  }

  .calculator-card {
    background: var(--card-background);
    border-radius: var(--border-radius);
    padding: var(--spacing-lg);
    box-shadow: var(--shadow-md);
    border: 1px solid var(--border-color);
  }

  .calculator-card h2 {
    font-size: 1.5rem;
    font-weight: 600;
    margin: 0 0 var(--spacing-xs) 0;
    color: var(--text-primary);
  }

  .card-description {
    color: var(--text-secondary);
    margin: 0 0 var(--spacing-lg) 0;
    font-size: 1rem;
  }

  .input-section {
    margin-bottom: var(--spacing-lg);
  }

  .input-section h3 {
    font-size: 1.125rem;
    font-weight: 500;
    margin: 0 0 var(--spacing-sm) 0;
    color: var(--text-primary);
  }

  .input-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: var(--spacing-md);
  }

  .input-group {
    display: flex;
    flex-direction: column;
    gap: var(--spacing-xs);
  }

  .input-group label {
    font-size: 0.875rem;
    font-weight: 500;
    color: var(--text-primary);
  }

  .number-input {
    padding: 0.75rem 1rem;
    border: 2px solid var(--border-color);
    border-radius: 8px;
    font-size: 1rem;
    background: var(--card-background);
    transition: all 0.2s ease;
  }

  .number-input:focus {
    outline: none;
    border-color: var(--primary-color);
    box-shadow: 0 0 0 3px var(--primary-light);
  }

  .number-input:hover {
    border-color: #9ca3af;
  }

  .result-section {
    margin-top: var(--spacing-lg);
    padding-top: var(--spacing-lg);
    border-top: 1px solid var(--border-color);
  }

  .result-card {
    background: linear-gradient(
      135deg,
      var(--primary-color),
      var(--primary-dark)
    );
    color: white;
    padding: var(--spacing-md);
    border-radius: var(--border-radius);
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: var(--spacing-xs);
    box-shadow: var(--shadow-lg);
  }

  .result-label {
    font-size: 0.875rem;
    font-weight: 500;
    opacity: 0.9;
  }

  .result-value {
    font-size: 1.75rem;
    font-weight: 700;
    font-variant-numeric: tabular-nums;
  }

  /* Responsive design */
  @media (max-width: 640px) {
    main {
      padding: var(--spacing-sm);
    }

    .header h1 {
      font-size: 2rem;
    }

    .calculator-card {
      padding: var(--spacing-md);
    }

    .input-grid {
      grid-template-columns: 1fr;
    }

    .tab-button {
      padding: 0.5rem 0.75rem;
    }

    .tab-label {
      display: none;
    }

    .tab-icon {
      font-size: 1.25rem;
    }
  }

  /* Mobile tab scrolling */
  @media (max-width: 480px) {
    .tab-nav {
      overflow-x: auto;
      scrollbar-width: none;
      -ms-overflow-style: none;
    }

    .tab-nav::-webkit-scrollbar {
      display: none;
    }
  }
</style>
