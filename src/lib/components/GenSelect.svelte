<script>
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();

  export let selectedGen = "1";

  function onChange(e) {
    selectedGen = e.target.value;
    dispatch("changegen", selectedGen);
  }
</script>

<div class="gen-select-wrap">
  <select
    class="gen-select"
    bind:value={selectedGen}
    on:change={onChange}
    aria-label="Filter by generation"
  >
    <option value="all">All generations</option>
    <option value="1">Generation 1</option>
    <option value="2">Generation 2</option>
    <option value="3">Generation 3</option>
    <option value="4">Generation 4</option>
    <option value="5">Generation 5</option>
    <option value="6">Generation 6</option>
    <option value="7">Generation 7</option>
    <option value="8">Generation 8</option>
    <option value="9">Generation 9</option>
  </select>
</div>

<style>

</style>
