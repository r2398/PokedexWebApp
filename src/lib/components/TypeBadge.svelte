<script>
  export let typeName = "Normal";

  const TYPE_COLORS = {
    Normal: ["#A8A77A", "#D8D8B0"],
    Fire: ["#EE8130", "#FFD1B8"],
    Water: ["#6390F0", "#BEE3FF"],
    Electric: ["#F7D02C", "#FFF4BF"],
    Grass: ["#7AC74C", "#DFF7C9"],
    Ice: ["#96D9D6", "#E6FBFA"],
    Fighting: ["#C22E28", "#FFDAD7"],
    Poison: ["#A33EA1", "#EEC6F0"],
    Ground: ["#E2BF65", "#F8ECCB"],
    Flying: ["#A98FF3", "#E9E0FF"],
    Psychic: ["#F95587", "#FFD5E2"],
    Bug: ["#A6B91A", "#EEF6C3"],
    Rock: ["#B6A136", "#E7D7B3"],
    Ghost: ["#735797", "#D7CFE6"],
    Dragon: ["#6F35FC", "#D8CCFF"],
    Dark: ["#705746", "#DAD0C8"],
    Steel: ["#B7B7CE", "#E6E6F0"],
    Fairy: ["#D685AD", "#FFE9F2"]
  };

  const colors = TYPE_COLORS[typeName] || ["#777", "#ddd"];
  const iconPath = `/images/type-icon-${typeName.toLowerCase()}.png`;
</script>

<span class="type-badge" style="--c1: {colors[0]}; --c2: {colors[1]}">
  <img src={iconPath} alt={typeName} on:error={(e) => (e.currentTarget.style.display = "none")} />
  <span class="type-label">{typeName.toUpperCase()}</span>
</span>

<style>
  /* handled by app.css */
</style>
