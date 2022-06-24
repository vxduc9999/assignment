<template>
	<div id="app">
		<HomeComponent />
	</div>
</template>

<script>
import HomeComponent from "./components/HomeComponent.vue";

export default {
	name: "App",
	components: {
		HomeComponent,
	},
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Oxanium:wght@200&display=swap");

body,
button {
	font-family: "Oxanium", cursive;
}
</style>
