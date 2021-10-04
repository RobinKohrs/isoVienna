<script>
  import { onMount, setContext, createEventDispatcher } from "svelte";
  import { mapbox, key } from "./mapbox.js";

  setContext(key, {
    getMap: () => map,
  });

  const dispatch = createEventDispatcher();
  export let lat;
  export let lon;
  export let zoom;

  let container;
  let map;
  let coordinates = [];

  function initMap() {
    map = new mapbox.Map({
      container,
      style: "mapbox://styles/mapbox/streets-v9",
      center: [lon, lat],
      zoom,
    });

    console.log("in initmap", map);

    dispatch("map", map);

    // return map;
  }

  onMount(() => {
    map = initMap();
  });
</script>

<div bind:this={container} />

<style>
  div {
    width: 100%;
    height: 100%;
  }
</style>
