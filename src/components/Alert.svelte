<script>
  export let showAlert = false;
  export let alert = {};
</script>

<div class="alert-container">
  {#if showAlert}
    <div class="alert-box {alert.type}">
      <p>{alert.messege}</p>
    </div>
  {/if}
</div>
