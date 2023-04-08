<script lang="ts">
  import { onMount, tick } from "svelte";
  import { score, play } from "./lib/store";

  let canvas: HTMLCanvasElement;
  let ctx: CanvasRenderingContext2D;
  let snakeHead = 20;
  let snakeX: number;
  let snakeY: number;
  let snakeSpeed: number = 5;
  let playGame;
  function addToScore() {
    score.update((n) => n + 1);
  }
  play.subscribe((value) => {
    playGame = value;
  });
  onMount(() => {
    ctx = canvas.getContext("2d");
    snakeX = canvas.width / 2 - snakeHead;
    snakeY = canvas.height / 2 - snakeHead / 2;
    drawAssets();
  });

  function drawAssets() {
    rectangle("#9ca3af", 0, 0, canvas.width, canvas.height);
    rectangle("#111827", snakeX, snakeY, snakeHead, snakeHead);
  }

  function moveSnake() {
    if (snakeX > canvas.width) {
      snakeX = 0;
      addToScore();
    }
    snakeX += snakeSpeed;
  }

  function rectangle(
    color: string,
    startX: number,
    startY: number,
    endX: number,
    endY: number
  ) {
    ctx.fillStyle = color;
    ctx.fillRect(startX, startY, endX, endY);
  }

  export async function gameLoop() {
    await tick();
    drawAssets();
    moveSnake();
    if (playGame) {
      requestAnimationFrame(gameLoop);
    }
    console.log(playGame, snakeSpeed);
  }
  //function handleClick() {
  //  play.update((n) => !n);
  //  gameLoop();
  //}
</script>

<canvas
  bind:this={canvas}
  width="600px"
  height="600px"
  class="border-8 border-indigo-600"
/>
<!-- <button -->
<!--   on:click={handleClick} -->
<!--   class="py-4 px-8 text-2xl outline rounded-lg {playGame -->
<!--     ? 'text-indigo-500 outline-indigo-700' -->
<!--     : 'text-sky-500 outline-sky-700'} ">Play</button -->
<!-- > -->
