<script>
  import MedalNumber from "./components/MedalNumber.svelte";
  import Title from "./components/TItle.svelte";
  import Instructions from "./components/Instructions.svelte";
  import Footer from "./components/Footer.svelte";
  import ModelRender from "./components/ModelRender.svelte";
  import Loading from "./components/Loading.svelte";

  let number = $state(0);
  let generating = $state(false);
</script>

<main class="container">
  <MedalNumber bind:value={number} bind:generating={generating} />
  {#if !generating && number !== 0}
  <ModelRender value={number}/>
  {:else if generating && number !== 0}
  <Loading bind:generating={generating} />
  {/if}
  <Instructions />
  <Footer />
</main>

<style>

  main {
    padding-top: 20px;
  }

</style>
