<script lang="ts">
  import { tweened } from "svelte/motion";
  import MoonIcon from "$lib/icons/MoonIcon.svelte";
  import SunIcon from "$lib/icons/SunIcon.svelte";
  import Bar, { type BarStatus } from "./Bar.svelte";

  export let min: number = 0;
  export let max: number = 1;
  export let optimum: number = 0.5;
  export let dynamic: boolean = true; // Set to true to enable dynamic behavior
  export let value: number = 0;
  export let duration: number = 2000;  // Set duration in ms

  // Use tweened for smooth interpolation of the value
  let dynamicValue = tweened(dynamic ? value : min);
  let direction = 1;

  let width: number;

  const bars = Array.from({ length: 7 }, (_, i) => ({
    highlighted: false,
    active: false,
    previous: false,
  }));

  const updateValue = async () => {
    let targetValue = $dynamicValue >= max ? min : max;
    direction = targetValue >= max ? -1 : 1;
    await dynamicValue.update(() => targetValue, {
      duration: duration,
    });

    updateValue(); // Recursively call the function for continuous animation
  };

  const updateBars = async () => {
    for (let i = 0; i < bars.length; i++) {
      const bar = bars[i];
      const isHighlighted = value >= i / bars.length * max;

      // Update the state of the current bar
      bar.highlighted = isHighlighted;
    }

    for (let i = 0; i < bars.length; i++) {
      if (bars[i].highlighted && !bars[i + 1]?.highlighted) {
        if (i != 0) {
          bars[i].active = true;
          bars[i - 1].previous = true;
        }
      }
    }
  };

  const barStatus = (index: number, currentValue: number): BarStatus => {
    const bar = bars[index];

    // If dynamic, use the animation logic
    if (dynamic) {
      const isHighlighted =
        currentValue >= index / bars.length * max && !(currentValue <= min);
      const isActive = isHighlighted && !bars[index + 1]?.highlighted;

      let isPrevious = false;

      if (direction < 0) {
        isPrevious = bars[index + 1]?.active;
      } else {
        isPrevious = bars[index - 1]?.active;
      }

      // Update the state of the current bar
      bar.active = isActive;
      bar.highlighted = isHighlighted;
      bar.previous = isPrevious;

      return {
        active: isActive,
        highlighted: isHighlighted,
        previous: isPrevious,
      };
    } else {
      return {
        active: bar.active,
        highlighted: bar.highlighted,
        previous: bar.previous,
      };
    }
  };

  updateBars();
  
  if (dynamic) {
    // Start the animation loop
    updateValue();
  }
</script>

<div bind:clientWidth={width} class="metter" class:small={width <= 320}>
  <MoonIcon color={$dynamicValue < optimum ? "#FFFFFF" : "#FFFFFF66"} />
  <div>
    <div class="bar-chart">
      {#each bars as _, index}
        <Bar status={barStatus(index, $dynamicValue)} small={width <= 320} />
      {/each}
    </div>
  </div>
  <SunIcon color={$dynamicValue >= optimum ? "#FFFFFF" : "#FFFFFF66"} />
</div>

<style lang="postcss">
  .metter {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 28px;
  }

  .metter.small {
    gap: 24px;
  }

  .bar-chart {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 10px;
    min-height: 42px;
  }
</style>
