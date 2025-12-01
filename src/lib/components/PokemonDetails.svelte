<script>
  import PokemonImageBox from "./PokemonImageBox.svelte";
  import TypeBadge from "./TypeBadge.svelte";
  import PokedexEntry from "./PokedexEntry.svelte";

  export let selectedDetails = null;
  export let loadingDetails = false;
  export let detailsError = null;

  const placeholder = {
    name: "Please select a Pokémon",
    dexNumber: "",
    dexEntry: "Select a Pokémon from the list on the left to view details here.",
    normalImage: "/images/pokeball-placeholder.svg",
    shinyImage: "/images/pokeball-placeholder.svg",
    types: []
  };

  function pokeHomeSpriteUrl(dex, shiny = false) {
    if (!dex) return null;
    const base =
      "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/home/";
    return shiny ? `${base}shiny/${dex}.png` : `${base}${dex}.png`;
  }

  $: normalImageUrl = selectedDetails
    ? selectedDetails.normalImage || pokeHomeSpriteUrl(selectedDetails.dexNumber, false)
    : placeholder.normalImage;

  $: shinyImageUrl = selectedDetails
    ? selectedDetails.shinyImage || pokeHomeSpriteUrl(selectedDetails.dexNumber, true)
    : placeholder.shinyImage;

  let audioEl;
  let isAnimationActive = true;
  let pokeballPosition = "46%";

  function playCry(event) {
    if (!audioEl) return;
    audioEl.currentTime = 0;
    audioEl.play().catch(() => {});

    // Stop the animation and set the Pokeball's position to where the user clicked
    isAnimationActive = false;
    const containerRect = event.currentTarget.parentElement.getBoundingClientRect();
    const clickX = event.clientX - containerRect.left;
    pokeballPosition = `${Math.min(
      Math.max(clickX - 20, 0), // Ensure the Pokeball stays within bounds
      containerRect.width - 40
    )}px`;
  }

  $: if (selectedDetails) {
    // Restart the animation when a new Pokémon is selected
    isAnimationActive = true;
    pokeballPosition = "50%"; // Reset the Pokeball to the center
  }
</script>

<section class="details-section">
  <div class="details-header">
    <h1 class="pokemon-title">{selectedDetails ? selectedDetails.name : placeholder.name}</h1>
    <span class="pokemon-id">
      {selectedDetails ? `#${String(selectedDetails.dexNumber).padStart(3, "0")}` : ""}
    </span>
  </div>

  <div class="details-divider"></div>

  <PokemonImageBox
    normalImage={normalImageUrl}
    shinyImage={shinyImageUrl}
    disabled={!selectedDetails}
    altText={selectedDetails ? selectedDetails.name : "placeholder"}
  />

  <div class="pokemon-types" style="margin-top:18px;">
    {#if loadingDetails}
      <!-- Show nothing for types while loading -->
    {:else if detailsError}
      <!-- Show nothing for types on error -->
    {:else if selectedDetails && selectedDetails.types?.length}
      {#each selectedDetails.types as t}
        <div style="display:inline-block;"><TypeBadge typeName={t} /></div>
      {/each}
    {:else}
      <p>No types available</p>
    {/if}
  </div>

  <PokedexEntry
  dexEntry={selectedDetails ? selectedDetails.dexEntry || "" : placeholder.dexEntry}
  loadingDetails={loadingDetails}
  detailsError={detailsError}
/>

  <div class="play-cry-container">
    <audio bind:this={audioEl} src={selectedDetails ? selectedDetails.crySound : ""}></audio>
    <button
      class="play-cry-btn"
      on:click={playCry}
      aria-label="Play Pokémon cry"
      disabled={!selectedDetails || !selectedDetails.crySound}
      class:is-stopped={!isAnimationActive}
      style="left: {pokeballPosition};"
    >
      <img src="/images/pokeballicon.png" alt="Pokeball Icon" class="pokeball-icon" />
    </button>
  </div>
</section>

<style>
  .details-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 10px;
  }

  .pokemon-title {
    font-size: 2rem;
    font-weight: 700;
    color: #3b4cca;
    letter-spacing: 3.5px;
    font-family: "Pokemon", "Roboto", Arial, sans-serif;
    margin-top: 0;
    margin-bottom: 0;
  }

  .pokemon-id {
    font-size: 1.1rem;
    color: #7c4dff;
    font-weight: 700;
    font-family: "Pokemon", "Roboto", Arial, sans-serif;
  }

  .pokemon-types p {
    font-size: 1rem;
    color: #666;
    font-style: italic;
    margin-top: 10px;
  }

  .play-cry-container {
    position: relative;
    width: 100%;
    height: 50px;
    margin-top: 20px;
    overflow: hidden;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .play-cry-btn {
    position: absolute;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    background: none;
    border: none;
    cursor: pointer;
    animation: moveSideToSide 3s infinite alternate ease-in-out;
    transition:
      transform 0.2s,
      box-shadow 0.2s;
  }

  .play-cry-btn.is-stopped {
    animation: none;
  }

  .play-cry-btn:hover {
    transform: scale(1.1);
  }

  .play-cry-btn:disabled {
    cursor: not-allowed;
    animation: none;
    opacity: 0.5;
  }

  .pokeball-icon {
    width: 45px;
    height: 45px;
  }

  @keyframes moveSideToSide {
    0% {
      left: 0;
    }
    100% {
      left: calc(100% - 40px);
    }
  }
</style>
