<script>
  import { onMount } from "svelte";
  import PokedexList from "$lib/components/PokedexList.svelte";
  import PokemonDetails from "$lib/components/PokemonDetails.svelte";
  import "$lib/css/app.css";

  const API_BASE = "https://pkserve.ocean.anhydrous.dev/api/pokedex";

  let pokemons = [];
  let filtered = [];
  let selectedDex = null;
  let selectedDetails = null;
  let searchText = "";
  let gen = "1";
  let loadingList = false;
  let loadingDetails = false;
  let listError = null;
  let detailsError = null;

  async function fetchList(selectedGen = "1") {
    loadingList = true;
    listError = null;
    pokemons = [];
    filtered = [];
    try {
      const url = `${API_BASE}?gen=${encodeURIComponent(selectedGen)}`;
      const res = await fetch(url);
      if (!res.ok) throw new Error(`List fetch failed (${res.status})`);
      const data = await res.json();
      pokemons = Array.isArray(data) ? data : [];
      pokemons.sort((a, b) => (a.dexNumber || 0) - (b.dexNumber || 0));
      applySearch();
    } catch (err) {
      listError = err.message;
    } finally {
      loadingList = false;
    }
  }

  async function fetchDetails(dex) {
    if (!dex) {
      selectedDetails = null;
      return;
    }
    loadingDetails = true;
    detailsError = null;
    selectedDetails = null;
    try {
      const res = await fetch(`${API_BASE}/${encodeURIComponent(dex)}`);
      if (!res.ok) throw new Error(`Details fetch failed (${res.status})`);
      selectedDetails = await res.json();
    } catch (err) {
      detailsError = err.message;
    } finally {
      loadingDetails = false;
    }
  }

  function applySearch() {
    const q = (searchText || "").trim().toLowerCase();
    if (!q) {
      filtered = pokemons.slice();
      return;
    }
    filtered = pokemons.filter((p) => {
      const name = (p.name || "").toLowerCase();
      const dex = String(p.dexNumber || "");
      return name.includes(q) || dex.includes(q);
    });
  }

  function onSelect(e) {
    selectedDex = e.detail;
    fetchDetails(selectedDex);
  }
  function onSearch(e) {
    searchText = e.detail;
    applySearch();
  }
  async function onChangeGen(e) {
    gen = e.detail;
    selectedDex = null;
    selectedDetails = null;
    await fetchList(gen);
  }

  onMount(async () => {
    await fetchList(gen);
  });
</script>

<!-- Use the same container class from Part A styles to replicate exact layout -->
<div class="container">
  <PokedexList
    {filtered}
    {selectedDex}
    {searchText}
    {gen}
    {loadingList}
    {listError}
    on:select={onSelect}
    on:search={onSearch}
    on:changegen={onChangeGen}
  />

  <main class="details-section">
    <PokemonDetails {selectedDetails} {loadingDetails} {detailsError} />
  </main>
</div>

<style>
  /* small override so Svelte dev layout doesn't conflict */
  .container {
    /* defined in app.css */
  }
</style>
