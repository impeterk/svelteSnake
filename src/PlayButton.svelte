<script lang="ts">
  import { play } from "./lib/store";
  let playGame: boolean;

  play.subscribe((value) => {
    playGame = value;
  });

  function handleClick() {
    play.update((n) => !n);
    console.log(playGame);
  }
  console.log(playGame);
</script>

<div class="flex">
  <button
    on:click={handleClick}
    class="py-4 px-8 text-2xl outline rounded-lg {playGame
      ? 'text-indigo-500 outline-indigo-700'
      : 'text-sky-500 outline-sky-700'} ">Play</button
  >
  <h3 class="text-2xl text-slate-400">{play ? "Playing" : "Stop"}</h3>
</div>
