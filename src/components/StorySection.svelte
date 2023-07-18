<script>
	import {onMount} from "svelte";
	import ProjectCarousel from "./ProjectCarousel.svelte";

	let blobContainer;
	let contentBox;
	let blobLocations = [];

	//Cursor location
	let cursorX = 0;
	let cursorY = 0;

	//Blob related variables
	const blobSize = 250; //Visual size must be changed in the CSS manually
	const spreadPower = 0.7; //How aggressively the blobs spread out evenly;
	const cursorRepulsionPower = 3; //How aggressively the blobs move away from the cursor

	onMount(async ()=> {
		//Make the background (blobs) the same size as the content.
		blobContainer.style.height = `${contentBox.clientHeight}px`;

		//Make a list containing the random locations of 200 blobs
		for (let i = 0; i < 100; i++) {
			blobLocations.push({
				x: Math.random() * blobContainer.clientWidth - 125,
				y: Math.random() * blobContainer.clientHeight - 125,
				id: i,
				element: null,
			})
		}
		blobLocations = blobLocations;
	})

	const blobAnimation = () => {
		//For each blob in bloblocations, find its distance to the cursor
		blobLocations.forEach((blob, index) => {
			//Get the center screen coordinate of each blob using getBoundingClientRect
			let blobX = blob.element.getBoundingClientRect().left + blobSize/2;
			let blobY = blob.element.getBoundingClientRect().top + blobSize/2;

			let newX;
			let newY;

			//Calculate the distance to the cursor
			let distance = Math.sqrt(Math.pow(blobX - cursorX, 2) + Math.pow(blobY - cursorY, 2));
			//If the distance is less than 150px, move the blob away from the cursor
			if (distance < 200) {
				let angle = Math.atan2(blobY - cursorY, blobX - cursorX);
				newX = blob.x + Math.cos(angle) * cursorRepulsionPower;
				newY = blob.y + Math.sin(angle) * cursorRepulsionPower;
			}else{
				//Find the blob's neighbors (within 2/3rds of its size)
				let neighbors = blobLocations.filter((blob2, index2) => {
					//The filtering function
					if (index2 !== index) {
						let distance = Math.sqrt(Math.pow(blob2.x - blob.x, 2) + Math.pow(blob2.y - blob.y, 2));
						if (distance < blobSize * (2/3)) {
							return true;
						}
					}
					return false;
				});
				//If there are neighbors, move away from them slowly
				if (neighbors.length > 0) {
					neighbors.forEach((neighbor) => {
						let angle = Math.atan2(blob.y - neighbor.y, blob.x - neighbor.x);
						newX = blob.x + Math.cos(angle) * spreadPower * (neighbors.length * 0.1);
						newY = blob.y +  Math.sin(angle) * spreadPower * (neighbors.length * 0.1);
					});
				}
			}
			//As long as the new coordinates are within the bounds of the container, update the blob's location
			if (newX + blobSize/2 > 0 && newX - blobSize/2 < blobContainer.clientWidth - blobSize) {
				blob.x = newX;
			}
			if (newY + blobSize/2 > 0 && newY - blobSize/2 < blobContainer.clientHeight - blobSize) {
				blob.y = newY;
			}
		})
		//Reassignment so Svelte updates the blobs in the DOM.
		blobLocations = blobLocations;
		window.requestAnimationFrame(blobAnimation);
	};
	window.requestAnimationFrame(blobAnimation);

	//Keep track of where the mouse is on the screen.
	const mouseMoveHandler = (event) => {
		cursorX = event.clientX;
		cursorY = event.clientY;
	}
</script>

<svelte:window on:mousemove={mouseMoveHandler}/>
<div class="container">
	<!--This div houses the interactive blobs in the background	-->
	<div class="background" bind:this={blobContainer}>
		<!--The blobs	-->
		{#each blobLocations as blob (blob.id)}
			<div class="ball" style="top:{blob.y}px; left:{blob.x}px" bind:this={blob.element}></div>
		{/each}
	</div>

	<div class="content" bind:this={contentBox}>
		<div class="card parallaxFast">
			<h1>Hi, I'm KimJammer</h1>
			<p>I'm a somebody who does stuff for some reason and stuff like that and other text that will go here.</p>
			<h1>A developer</h1>
		</div>
		<div class="card parallaxFast">
			<h1>Across Languages,</h1>
			<p>Some body text about some part of my life that is hopefully interesting or something i dont know lol.</p>
		</div>
		<div class="card parallaxFast">
			<h1>Over Borders,</h1>
			<p>Some body text about some part of my life that is hopefully interesting or something i dont know lol.</p>
		</div>
		<div class="card parallaxFast">
			<h1>& Fueled by Curiosity</h1>
			<p>Some body text about some part of my life that is hopefully interesting or something i dont know lol.</p>
		</div>
	</div>
</div>

<style lang="scss">
	.container {
		margin-top: 5vh;
	}
	.content {
		display: grid;

		gap: 5em;
		box-sizing: border-box;
		padding: 2em;
		margin-top: -5vh;
		margin-bottom: 25vh;
	}
	.card {
		background-color: #EDF5FF7F;
		z-index: 10;
		padding: 1em;
		border-radius: 1em;
		width: 90%;
		justify-self: center;

		transition: 0.5s ease;
		box-shadow: 0px 0px 10px 0px rgba(0,0,0,0.75);
	}
	.card:hover{
		scale: 1.008;
		box-shadow: 4px 4px 13px 0px rgba(0,0,0,0.75);
	}

	@media (min-width:1025px) {
		.card{
			width: 50%;
		}
		.card:nth-child(1) {
			grid-column: 1/2;
			grid-row: 1/3;
			justify-self: end;
		}
		.card:nth-child(2) {
			grid-column: 2/end;
			grid-row: 2/4;
			justify-self: start;
		}
		.card:nth-child(3) {
			grid-column: 1/2;
			grid-row: 3/5;
			justify-self: end;
		}
		.card:nth-child(4) {
			grid-column: 2/end;
			grid-row: 4/6;
			justify-self: start;
		}
	}


	.background {
		position:absolute;
		width: 100%;
		z-index:-35;
		overflow-x: clip;
		filter: blur(30px);
	}
	.ball{
		background-color: #596be3;
		position: absolute;
		border-radius: 50%;
		width: 250px;
		height: 250px;
		z-index: -50;
	}
</style>