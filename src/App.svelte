<script>
  import MedalNumber from "./components/MedalNumber.svelte";
  import Title from "./components/TItle.svelte";
  import Instructions from "./components/Instructions.svelte";
  import Footer from "./components/Footer.svelte";
  import ModelRender from "./components/ModelRender.svelte";

  let number = $state(0);
</script>

<main class="container">
  <Title />
  <ModelRender value={number}/>
  <MedalNumber bind:value={number} />
  <Instructions />
  <Footer />
</main>

<style>
</style>
