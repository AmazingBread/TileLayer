<template>
	<div id="map"></div>
</template>

<script>
import L from "leaflet";
import "leaflet/dist/leaflet.css";
import trackData from "@/assets/trackData.json";

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
		this.drawPolyLine();
	},
	methods: {
		init() {
			this.map = new L.map("map").setView([51.505, -0.09], 13);
			L.tileLayer("https://tile.openstreetmap.org/{z}/{x}/{y}.png", {
				maxZoom: 19,
				attribution:
					'&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>',
			}).addTo(this.map);
		},
		drawPolyLine() {
			/* data 순서 맞추기 */
			trackData.sort(function (a, b) {
				if (a.updated > b.updated) {
					return 1;
				}
				if (a.updated < b.updated) {
					return -1;
				}
				// a must be equal to b
				return 0;
			});

			let coordinates = [];
			let firstLng = parseFloat(trackData[0].longitude)
			let lastLng = parseFloat(trackData[trackData.length-1].longitude)
			let abs = firstLng - lastLng
			
			if(Math.abs(abs) > 180 || (firstLng < 0 && lastLng < 0)){
				trackData.forEach((coordinate) => {
					let lng = parseFloat(coordinate.longitude);
					if(lng < 0){
						lng += 360
					}
					coordinates.push([parseFloat(coordinate.latitude), lng]);
				})
			} else {
				trackData.forEach((coordinate) => {
					let lng = parseFloat(coordinate.longitude);
					if(firstLng < 0){
						lng += 360
					}
					coordinates.push([parseFloat(coordinate.latitude), lng]);
				})

			}
			var polyline = L.polyline(coordinates, { color: "red" }).addTo(
				this.map
			);
			// zoom the map to the polyline
			this.map.fitBounds(polyline.getBounds());
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
