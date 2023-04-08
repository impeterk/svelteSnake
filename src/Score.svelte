<script lang="ts">
  import { fly } from "svelte/transition";
  import { score } from "./lib/store";
  let scoreCount: number;

  score.subscribe((value) => {
    scoreCount = value;
  });

</script>

<h2
  transition:fly="{{ y: 200, duration: 2000 }}"
  class="text-gray-200 text-5xl font-semibold"
>
  {scoreCount}
</h2>
