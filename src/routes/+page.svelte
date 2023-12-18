<script lang="ts">
	import '../app.css';
	import type { LatLngExpression } from 'leaflet';
	import Leaflet from '$lib/Leaflet.svelte';
  import MapInfo from '$lib/MapInfo.svelte'; 
	import Marker from '$lib/Marker.svelte';
	import Popup from '$lib/Popup.svelte';

	const initialView: LatLngExpression = [41.3161, -110.3837]; // Fort Bridger
	const markerLocations: Array<LatLngExpression> = [
		[41.317951, -110.38971] // Jim Bridger Rondezvous
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
		<MapInfo initialView={initialView} />
	</Leaflet>
</div>
