<!-- MapInfo.svelte -->
<script lang="ts">
  import { onMount, onDestroy, getContext, setContext } from 'svelte';
  import L from 'leaflet';

  export let currentViewLocation: L.LatLngExpression | undefined;
  export let initialView: L.LatLngExpression; // Define initialView as a prop


  console.log('initialView', initialView);

  const { getMap }: { getMap: () => L.Map | undefined } = getContext('map');

  // L.MapInfo inherits from L.Control
  setContext('control', {
    // L.MapInfo inherits from L.Control
    getControl: () => currentViewLocation,
  });

  let map: L.Map | undefined;
  let reticle: L.Marker | undefined;

  onMount(() => {
    map = getMap();

    if (map) {
      // Add a move event listener to update currentViewLocation as the map view changes
      map.on('move', () => {
        currentViewLocation = map.getCenter();
      });

      // Add the MapInfo control to the map's bottom-left corner
      map.addControl(L.control({ position: 'bottomleft' }));

      // Initialize currentViewLocation with the initial center
      currentViewLocation = map.getCenter();

    
    }
  });

  onDestroy(() => {
    if (map) {
      map.off('move'); // Remove the move event listener when the component is destroyed
    
    }
  });

  function copyCoordinates() {
    if (currentViewLocation) {
      const coords = `Lat: ${currentViewLocation.lat.toFixed(4)}, Lng: ${currentViewLocation.lng.toFixed(4)}`;
      navigator.clipboard.writeText(coords)
        .then(() => {
          console.log('Coordinates copied to clipboard');
        })
        .catch(err => {
          console.error('Error in copying text: ', err);
        });
    }
  }


</script>

<div class="map-info">
  <p>
    Current View Location: {
      currentViewLocation 
        ? `Lat: ${currentViewLocation.lat.toFixed(4)}, Lng: ${currentViewLocation.lng.toFixed(4)}` 
        : `Lat: ${initialView[0]}, Lng: ${initialView[1]}`
        }
  </p>
  <div class="reticle">+</div>
  <button on:click={copyCoordinates}>Copy Coordinates</button>

</div>

<style>
  .map-info {
    position: fixed;
    bottom: 0;
    left: 0;
    z-index: 1000;
    background-color: white;
    padding: 10px;
  }
  button {
    /* Styling properties */
    padding: 5px 10px;
    margin-top: 10px;
    cursor: pointer;
    /* Add more styling as needed */
  }
  .reticle {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    font-size: 24px;
    color: black;
    pointer-events: none; /* Ensures click events pass through */
    z-index: 1000; /* Ensure it's above the map layers */
  }

</style>
