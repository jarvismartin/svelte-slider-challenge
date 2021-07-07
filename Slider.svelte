<script>
	export let photos;

	import { slide } from "svelte/transition";
	import { quintOut } from "svelte/easing";
	import { crossfade } from "svelte/transition";

	// Hold the value for the current image
	// i.e. photos[i]
	let i = 0;

	// Hold previous and next image i values
	let prev;
	function getPrev(n) {
	  return (prev = n == 0 ? photos.length - 1 : n - 1);
	}

	let next;
	function getNext(n) {
	  return (next = n == photos.length - 1 ? 0 : n + 1);
	}

	let change = "";

	function changeImg(direction) {
	  // Handle the motion of changing slides
	  change = direction;

	  // Increment / Decrement the current photo
	  i = direction === "forward" ? i++ : i--;
	  if (direction === "forward") {
	    i++;
	  } else {
	    i--;
	  }

	  // Handle wrapping around the list of photos
	  // i.e. clicking "backward" when photos[0]
	  if (i < 0) {
	    i = photos.length - 1;
	  }

	  if (i > photos.length - 1) {
	    i = 0;
	  }

	  // Update prev and next
	  getPrev(i);
	  getNext(i);
	}
</script>

<style>
	.slider {
	  background-color: transparent;
	  color: #333;
	  flex: 1; /* ASSUMES display: flex; in parent */
	  display: grid;
	  grid-template-columns: 50px auto 50px;
	  grid-template-rows: 100px auto 50px;
	  grid-template-areas:
	    "title title title"
	    "middle middle middle"
	    "footer footer footer";
	  padding: 15px;
	  /* Prevent user clicks from selecting controls */
	  user-select: none;
	}

	.title {
	  grid-area: title;
	  font-size: 1.5rem;
	  overflow: auto;
	  word-wrap: wrap;
	  margin-top: 15px;
	}

	.middle {
	  grid-area: middle;
	  display: flex;
	  justify-content: space-between;
	  align-items: center;
	  align-self: stretch;
	  flex: 1;
	  margin: 10px 0;
	  height: 400px;
	}

	.slide {
	  /* 
					  display: flex;
					  flex-direction: column;
					  align-items: center;
					  justify-content: center;
						*/
	}

	.slide img {
	  flex: 1;
	  border-radius: 7px;
	  object-fit: contain;
	  padding: 2px;
	  border: 2px solid gray;
	  max-width: 600px;
	  max-height: 300px;
	}

	.control {
	  width: 50px;
	  height: 50px;
	  cursor: pointer;
	  opacity: 0.5;
	  border-radius: 50%;
	  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
	  transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1);
	  display: flex;
	}

	.control:hover {
	  opacity: 0.65;
	  box-shadow: 0 14px 28px rgba(0, 0, 0, 0.25), 0 10px 10px rgba(0, 0, 0, 0.22);
	}

	.control:active {
	  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
	}

	.control .left {
	  transform: rotate(180deg);
	}

	details {
	  grid-area: footer;
	  cursor: pointer;
	  align-self: flex-start;
	  text-align: left;
	}

	summary {
	  font-size: 1.2rem;
	}

	.details-content {
	  position: absolute;
	  background-color: #333;
	  color: #bdbdbd;
	  padding: 15px;
	  border: 2px solid black;
	  border-radius: 7px;
	  margin-left: 130px;
	  margin-top: -200px;
	}

	info {
	  display: flex;
	  align-items: center;
	}

	.section-label {
	  font-weight: bold;
	  margin-right: 15px;
	  width: 142px;
	  text-align: right;
	}
</style>

<div class="slider">
	{#if photos.length > 0}

		<header class="title">{photos[i].data[0].title}</header>

		<div class="middle">

			<!-- Backward Control -->
			<div class="control" on:click="{() => changeImg('backward')}">
				<img class="left" src="control.png" alt="go backward" />
			</div>

			<!-- IMAGES -->
			<div class="slide">
	 	 		<img 
					src="{photos[i].links[0].href}" 
					alt="{photos[i].data[0].description}" 
					transition:slide="{{delay: 250, duration: 300, easing: quintOut }}"
				/>
			</div>


			<!-- Forward Control -->
			<div class="control" on:click="{() => changeImg('forward')}">
				<img src="control.png" alt="go forward" />
			</div>

		</div>

		<details class="footer">
			<summary>More...</summary>
			<div class="details-content">
				<info>
					<div class="section-label">photographer:</div> 
					<p>
						{photos[i].data[0].photographer}
					</p>
				</info>
				<info>
					<div class="section-label">date created:</div> 
					<p>
						{photos[i].data[0].date_created}
					</p>
				</info>
				<info>
					<div class="section-label">location:</div> 
					<p>
						{photos[i].data[0].location}
					</p>
				</info>
				<info>
					<div class="section-label">description:</div> 
					<p>
						{photos[i].data[0].description}
					</p>
				</info>
			</div>
		</details>
	{/if}

</div>