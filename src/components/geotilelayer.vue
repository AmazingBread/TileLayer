<template>
	<div id="map"></div>
</template>

<script>
/* eslint-disable */
import L from "leaflet";
import "leaflet/dist/leaflet.css";
import "./L.TileLayer.BetterWMS.js";

export default {
	name: "HelloWorld",
	props: {
		msg: String,
	},
	data() {
		return {
			map: null,
		};
	},
	mounted() {
		this.init();
	},
	methods: {
		init() {
			this.map = new L.map("map").setView([31.505, 50.09], 3);
			L.tileLayer("https://tile.openstreetmap.org/{z}/{x}/{y}.png", {
				minZoom: 2,
				maxZoom: 15,
				attribution:
					'&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>',
			}).addTo(this.map);


			let LayerUrl = 'https://map.ocean-look.com/oceanlook/vessel/wms/GetMap',
			accessLayer = 'lvi_prm_sample1',
			layerStyles = {
				vessel: 'vessel:vessel_circle'
			}
			
			let wmsPane = this.map.createPane('wmsPane');
			wmsPane.style.zIndex = 250;

			let vesselAllLayer = L.tileLayer.betterWms(LayerUrl, {
				layers: accessLayer, //custom  lvi_prm lvi_latest_prm
				viewparams: 'token:5a33e87e3cb40480ce07d9bd6f0e8579880bbd6d40902f0afa73d8caface86b2',
				SRS: 'EPSG:4326',
				tileSize: 512,
				styles: layerStyles.vessel,
				format: 'image/png',
				transparent: true,
				minZoom: 2,
				maxZoom: 15,
				pane: wmsPane,
			})
			// .addTo(this.map)
			vesselAllLayer.addTo(this.map);
		},
		vesselSelect(test){
			alert(test.mmsi)
		},
		
	},
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
body {
	margin: 0;
}
html {
	margin: 0;
	/* min-width: 320px; */
}
#map {
	height: 100vh;
	z-index: 99;
}
</style>
