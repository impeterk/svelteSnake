<script lang="ts">
  import { onMount, tick } from "svelte";
  import { score, play } from "./lib/store";

  let canvas: HTMLCanvasElement;
  let ctx: CanvasRenderingContext2D;
  let snakeHead = 15;
  let snakeX: number;
  let snakeY: number;
  let snakeSpeed: number = 15;
  let playGame;
  let velocityX = 0;
  let velocityY = 0;
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
  function moveSnake() {
    snakeX += snakeHead * velocityX;
    snakeY += snakeHead * velocityY;
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

  function handleArrows(event: KeyboardEvent | TouchEvent) {
    if (event instanceof KeyboardEvent) {
      let key = event.key.toLowerCase();
      switch (key) {
        case "arrowright":
        case "l":
          console.log("moving Right");
          velocityY = 0;
          velocityX = 1;
          break;
        case "arrowup":
        case "k":
          console.log("moving Up");
          velocityX = 0;
          velocityY = -1;
          break;
        case "arrowdown":
        case "j":
          console.log("moving Down");
          velocityX = 0;
          velocityY = 1;
          break;
        case "arrowleft":
        case "h":
          console.log("moving Left");
          velocityY = 0;
          velocityX = -1;
          break;
        case " ":
          handleClick();
          break;
        default:
          console.log(key);
          console.log("not pressing movement key");
          break;
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
    let fps = 1000 / snakeSpeed;
    await tick();
    drawAssets();
    moveSnake();
    if (playGame) {
      setTimeout(() => {
        gameLoop();
      }, fps);
    }
  }
  function handleClick() {
    play.update((n) => !n);
    gameLoop();
  }
</script>

<svelte:window on:keydown={handleArrows} />
<canvas
  bind:this={canvas}
  width="600px"
  height="600px"
  class="border-8 border-lime-600 rounded-lg bg-gray-900"
/>
<slot />
<div class="flex">
  <button
    on:click={handleClick}
    class="py-4 px-8 text-2xl outline rounded-lg {playGame
      ? 'text-indigo-500 outline-indigo-700'
      : 'text-sky-500 outline-sky-700'} ">Play</button
  >
  <h3 class="text-2xl text-slate-400">{playGame ? "Playing" : "Stop"}</h3>
</div>
