<script>
  import Map from "./Map.svelte";
  import mapboxgl from "mapbox-gl";
  mapboxgl.accessToken =
    "pk.eyJ1Ijoicm9iaW5rbyIsImEiOiJja3VkMDZzYzYxNXM3MndvZGI3bHhqd2Q2In0.W2MkF3K0Fhs0NU_zbVO5pg";

  const urlBase = "https://api.mapbox.com/isochrone/v1/mapbox/";

  const profile = "cycling"; // Set the default routing profile
  const minutes = 10; // Set the default duration
  let coords = {};
  let map;
  let id = 0;

  function getIso(event) {
    map = event.detail;

    map.on("click", async (e) => {
      // check if there is already an isochrones layer
      // let maplayer = map.getLayer("isochrones");
      // if (typeof maplayer !== "undefined") {
      //   map.removeLayer(maplayer).removeSource("iso");
      // }

      let coords = e.lngLat.wrap();
      const query = await fetch(
        `${urlBase}${profile}/${coords.lng},${coords.lat}?contours_minutes=${minutes}&polygons=true&access_token=${mapboxgl.accessToken}`,
        { method: "GET" }
      );

      const data = await query.json();

      map.addSource("iso", {
        type: "geojson",
        data: data,
      });

      map.addLayer({
        id: "isochrones",
        type: "fill",
        source: "iso",
        paint: {
          "fill-color": "#222",
          "fill-opacity": 0.2,
        },
      });
    });
  }
</script>

<Map lat={48.2} lon={16.37} zoom={13} on:map={getIso} />
