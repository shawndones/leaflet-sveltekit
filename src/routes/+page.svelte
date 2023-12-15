<script lang="ts">
	import '../app.css';
	import type { LatLngExpression } from 'leaflet';
	import Leaflet from '$lib/Leaflet.svelte';
	import Marker from '$lib/Marker.svelte';
	import Popup from '$lib/Popup.svelte';
 

	const initialView: LatLngExpression = [51.514244, 7.468429]; // Dortmund, Germany
	const markerLocations: Array<LatLngExpression> = [
		[51.513870009926, 7.473969975241] // ShipBit Office
	];
</script>

<div class="w-full h-screen">
	<Leaflet view={initialView} zoom={14}>
		{#each markerLocations as latLng}
			<Marker {latLng} width={40} height={40}>
				<Popup
					>Popup Text</Popup
				>
			</Marker>
		{/each}
    
	</Leaflet>
</div>
