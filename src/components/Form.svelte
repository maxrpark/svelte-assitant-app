<script>
  import { getContext } from 'svelte';
  export let task = '';
  const handleSubmit = getContext('handleSubmit');
</script>

<form type="submit" on:submit={handleSubmit} class="form-container">
  <input
    bind:value={task}
    type="text"
    class="input-control"
    placeholder="Type something..."
    maxlength="25"
  />
</form>
