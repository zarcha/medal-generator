<script>
  import Medal from "./components/Medal.svelte";
  import MedalNumber from "./components/MedalNumber.svelte";
  import Title from "./components/TItle.svelte";
  import Instructions from "./components/Instructions.svelte";
  import Footer from "./components/Footer.svelte";
  let number = $state(0);
</script>

<main class="container">
  <Title />
  <Medal bind:value={number} />
  <MedalNumber bind:value={number} />
  <Instructions />
  <Footer />
</main>

<style>
</style>
