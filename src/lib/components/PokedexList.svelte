<script>
  import { createEventDispatcher } from "svelte";
  import GenSelect from "./GenSelect.svelte";
  const dispatch = createEventDispatcher();

  export let filtered = [];
  export let selectedDex = null;
  export let searchText = "";
  export let gen = "1";
  export let loadingList = false;
  export let listError = null;

  let localSearch = searchText;

  function selectPokemon(dex) {
    dispatch("select", dex);
  }
  function handleInput(e) {
    localSearch = e.target.value;
    dispatch("search", localSearch);
  }
  function handleGenChange(e) {
    dispatch("changegen", e.detail);
  }
</script>

<aside class="pokemon-list-section">
  <h2 class="pokedex-title">Pokedex</h2>

  <div class="search-box">
    <input
      type="text"
      placeholder="Search Pokemon..."
      bind:value={localSearch}
      on:input={handleInput}
      aria-label="Search Pokemon"
    />
  </div>

  <GenSelect bind:selectedGen={gen} on:changegen={handleGenChange} />

  <div class="list-wrap">
    {#if loadingList}
      <div class="list-loading">Loading Pokédex…</div>
    {:else if listError}
      <div class="list-error">Error: {listError}</div>
    {:else if filtered.length === 0}
      <div class="list-empty">No Pokémon found</div>
    {:else}
      <ul class="pokemon-list" role="list">
        {#each filtered as p (p.dexNumber)}
          <li class="pokemon-list-item {p.dexNumber === selectedDex ? 'selected' : ''}">
            <button
              type="button"
              class="pokemon-list-btn"
              aria-pressed={p.dexNumber === selectedDex}
              aria-label={`Select ${p.name}`}
              on:click={() => selectPokemon(p.dexNumber)}
              on:keydown={(e) => (e.key === "Enter" || e.key === " ") && selectPokemon(p.dexNumber)}
              tabindex="0"
            >
              <span class="pokemon-number">#{String(p.dexNumber).padStart(3, "0")}</span>
              <span class="pokemon-name">{p.name}</span>
            </button>
          </li>
        {/each}
      </ul>
    {/if}
  </div>
</aside>

<style>
  /* handled by app.css */
</style>
