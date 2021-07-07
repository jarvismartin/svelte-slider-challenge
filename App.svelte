<script>
	import { onMount } from "svelte";
	import Slider from "./Slider.svelte";

	let photos = [];

	onMount(async () => {
	  const res = await fetch("https://images-api.nasa.gov/search?q=rocketship");
	  photos = (await res.json()).collection.items;
	  console.log("photos:", photos);
	});
</script>

<style>
	main {
	  display: flex;
	  flex-direction: column;
	  font-family: sans-serif;
	  text-align: center;
	  color: #ddd;
	}

	.container {
	  align-self: center;
	  width: 700px;
	  height: 600px;
	  display: flex;
	  background-color: rgba(255, 255, 255, 0.65);
	  border-radius: 7px;
	}
</style>

<main>
	<h1>Out of This World</h1>
	<h2>NASA Slide Show</h2>
	<div class="container">
		<Slider {photos} />
	</div>
</main>