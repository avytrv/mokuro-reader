<script lang="ts">
  export let files: FileList | undefined = undefined;
  export let onUpload: ((files: FileList) => void) | undefined = undefined;

  let input: HTMLInputElement;

  function handleChange() {
    if (files && onUpload) {
      onUpload(files);
    }
  }

  function onClick() {
    input.click();
  }
</script>

<input
  type="file"
  bind:files
  bind:this={input}
  onchange={handleChange}
  {...$$restProps}
  class="hidden"
/>

<button class="hover:underline dark:text-primary-500" onclick={onClick}><slot>Upload</slot></button>
