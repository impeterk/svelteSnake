<script lang="ts">
  import { onMount } from "svelte";

  let canvas: HTMLCanvasElement;
  let ctx: CanvasRenderingContext2D;

  onMount(() => {
    ctx = canvas.getContext("2d");
    ctx.fillStyle = "#9ca3af";
    ctx.fillRect(0, 0, canvas.width, canvas.height);
  });
</script>

<canvas
  bind:this={canvas}
  width="600px"
  height="600px"
  class="border-8 border-indigo-600"
/>
