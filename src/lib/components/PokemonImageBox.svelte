<script>
  // images and disabled flag
  export let normalImage = "/images/pokeball-placeholder.svg";
  export let shinyImage = "/images/pokeball-placeholder.svg";
  export let disabled = false;
  export let altText = "pokemon";

  let isShiny = false;
  $: if (disabled) isShiny = false;
</script>

<!-- styles from Part A -->
<div class="pokemon-image-box {isShiny ? 'is-shiny' : ''} {disabled ? 'disabled' : ''}">
  <label class="shiny-toggle" aria-label="Toggle shiny image">
    <input type="checkbox" class="visually-hidden" bind:checked={isShiny} {disabled} />
    <img src="/images/star-64.png" alt="toggle shiny" class="shiny-star" />
    <span class="star-tooltip">show/hide shiny image</span>
  </label>

  <!-- Images using same classes as Part A for CSS transitions to match -->
  <img src={normalImage} alt={altText} class="pokemon-img normal-img" draggable="false" />
  <img src={shinyImage} alt={altText + " shiny"} class="pokemon-img shiny-img" draggable="false" />
</div>

<style>
  /* the main visual rules live in app.css (from Part A) */
  /* add a small rule so when is-shiny class present we show shiny image in browsers without :has support */
  .pokemon-image-box.is-shiny .normal-img {
    opacity: 0;
  }
  .pokemon-image-box.is-shiny .shiny-img {
    opacity: 1;
  }
</style>
