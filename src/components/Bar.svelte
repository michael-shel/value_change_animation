<script context="module" lang="ts">
  export type BarStatus = {
    active: boolean;
    previous: boolean;
    highlighted: boolean;
  };
</script>

<script lang="ts">
  export let status: BarStatus = {
    active: false,
    previous: false,
    highlighted: false,
  };
  export let small: boolean = false;
</script>

<div
  class:active={status.active}
  class:previous={status.previous}
  class:highlighted={status.highlighted}
  class="bar {small && 'small'}"
/>

<style lang="postcss">
  .bar {
    width: 8px;
    height: 24px;
    border-radius: 3px;
    background: #ffffff66;
    transition: height 0.5s ease, background 0.3s ease;
  }

  .bar:not(.active):not(.small) {
    border-right: 1px solid transparent;
    border-left: 1px solid transparent;
  }

  .bar.small {
    width: 6px;
    height: 20px;
  }

  .bar.highlighted {
    box-shadow: 0px 0px 24px -2px #ffffff;
    box-shadow: 0px 0px 6px -1px #ffffff;
    background: #ffffff;
  }

  .bar.active {
    width: 10px;
    height: 42px;
  }

  .bar.active.small {
    width: 6px;
    height: 36px;
  }

  .bar.previous {
    height: 28px;
  }

  .bar.previous.small {
    height: 24px;
  }
</style>
