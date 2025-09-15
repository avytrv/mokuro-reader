<script lang="ts">
  import { toggleFullScreen } from '$lib/panzoom';
  import { isReader } from '$lib/util';

  import { Button } from 'flowbite-svelte';

  export let open = true;

  function onClose() {
    open = false;
    history.back();
  }
</script>

{#if isReader()}
  <div class="flex flex-col gap-2">
    <Button color="alternative" onclick={toggleFullScreen}>Toggle fullscreen</Button>
    <Button color="alternative" onclick={onClose}>Close reader</Button>
  </div>
{/if}
