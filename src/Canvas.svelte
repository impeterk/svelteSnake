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
    snakeX = canvas.width / 2;
    snakeY = canvas.height / 2;
    drawAssets();
  });

  function drawAssets() {
    drawTiles();
    rectangle("#111827", snakeX, snakeY, snakeHead, snakeHead);
  }

  function drawTiles() {
    for (let i = 0; i <= canvas.width / snakeHead; i++) {
      for (let j = 0; j <= canvas.height / snakeHead; j++) {
        rectangle(
          "#f5f3ff",
          i * snakeHead,
          j * snakeHead,
          snakeHead - 1,
          snakeHead - 1
        );
      }
    }
  }
  function moveSnake(event: KeyboardEvent | TouchEvent) {
    if (event instanceof KeyboardEvent) {
      let key = event.key.toLowerCase();
      switch (key) {
        case "arrowright":
          console.log("moving Right");
          snakeX += snakeHead;
          break;
        case "arrowup":
          console.log("moving Up");
          snakeY -= snakeHead;
          break;
        case "arrowdown":
          console.log("moving Down");
          snakeY += snakeHead;
          break;
        case "arrowleft":
          console.log("moving Left");
          snakeX -= snakeHead;
          break;
        default:
          console.log(key);
          console.log("not pressing movement key");
          break;
      }
      if (snakeX >= canvas.width) {
        snakeX = 0;
      }
      if (snakeY >= canvas.width) {
        snakeY = 0;
      }
      if (snakeX < 0) {
        snakeX = canvas.width - snakeHead;
      }
      if (snakeY < 0) {
        snakeY = canvas.height - snakeHead;
      }
    }
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
    moveSnake;
    if (playGame) {
      requestAnimationFrame(gameLoop);
    }
  }
  function handleClick() {
    play.update((n) => !n);
    gameLoop();
  }
</script>

<svelte:window on:keydown={moveSnake} />
<canvas
  bind:this={canvas}
  width="600px"
  height="600px"
  class="border-8 border-lime-600 bg-gray-900"
/>
<slot />
<button
  on:click={handleClick}
  class="py-4 px-8 text-2xl outline rounded-lg {playGame
    ? 'text-indigo-500 outline-indigo-700'
    : 'text-sky-500 outline-sky-700'} ">Play</button
>
