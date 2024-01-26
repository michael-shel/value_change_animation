<!-- YourPage.svelte -->

<script lang="ts">
  import Meter from "$components/Meter.svelte";
</script>

<div class="meter-container">
  <Meter min={0} max={100} optimum={50} value={50} dynamic={true} />
</div>

<style lang="postcss">
  .meter-container {
	margin: 10%;
  }
</style>
