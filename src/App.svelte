<script lang="ts">
  import Canvas from "./Canvas.svelte";
  import PlayButton from "./PlayButton.svelte";
  import Score from "./Score.svelte";
  import svelteLogo from "./assets/svelte.svg";
  import viteLogo from "/vite.svg";
</script>

<main class="flex flex-col items-center">
  <div class="flex w-full justify-around">
    <a href="https://vitejs.dev" target="_blank" rel="noreferrer">
      <img src={viteLogo} class="aspect-square h-28" alt="Vite Logo" />
    </a>
    <Score />
    <a href="https://svelte.dev" target="_blank" rel="noreferrer">
      <img src={svelteLogo} class="aspect-square h-28" alt="Svelte Logo" />
    </a>
  </div>
  <h1 class="m-8 font-bold text-5xl text-gray-200">Vite + Svelte</h1>
  <Canvas />
  <PlayButton />
</main>

<style>
</style>
