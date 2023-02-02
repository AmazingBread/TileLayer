<template>
	<Meridian />
</template>

<script>
import Meridian from "./components/meridian.vue";

export default {
	name: "App",
	components: {
		Meridian,
	},
};
</script>

<style>
#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;
	/* margin-top: 60px; */
}
</style>
