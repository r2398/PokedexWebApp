<script>
  export let dexEntry = "";
  export let loadingDetails = false;
  export let detailsError = null;
</script>

<div class="pokemon-entry" style="width:100%;">
  <strong>Pokedex Entry</strong>
  {#if loadingDetails}
    <p>Loading details…</p>
  {:else if detailsError}
    <p class="error">Error loading details: {detailsError}</p>
  {:else}
    <p>{@html dexEntry.replace(/\n/g, "<br/>")}</p>
  {/if}
</div>

<style>
  /* in app.css */
</style>
