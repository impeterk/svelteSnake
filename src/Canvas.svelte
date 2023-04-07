<script lang="ts">
  const canvas = document.querySelector("canvas") as HTMLCanvasElement;
  const ctx = canvas.getContext("2d") as CanvasRenderingContext2D;

  ctx.fillStyle = "#9ca3af";
  ctx.fillRect(0, 0, canvas.width, canvas.height);
</script>

<canvas width="600px" height="600px" class="border-8 border-indigo-600" />
