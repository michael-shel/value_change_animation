<!-- YourPage.svelte -->

<script lang="ts">
  import Meter from "$components/Meter.svelte";
</script>

<div class="container">
  <div class="small">
    <Meter min={0} max={10} optimum={3} value={1} dynamic={true} />
  </div>
</div>

<Meter min={0} max={100} optimum={50} value={50} dynamic={true} duration={5000} />

<style lang="postcss">
  .container {
    align-items: center;
    display: flex;
    justify-content: center;
    flex-direction: column;
    margin: 3rem 0;
  }
</style>
