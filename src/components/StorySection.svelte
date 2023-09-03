<script>
	import {onMount} from "svelte";

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
	let blobContainerWidth = 0;
	let blobContainerHeight = 0;

	onMount(async ()=> {
		//Make the background (blobs) the same size as the content.
		blobContainer.style.height = `${contentBox.clientHeight}px`;
		blobContainerWidth = blobContainer.clientWidth;
		blobContainerHeight = blobContainer.clientHeight;

		//Make a list containing the random locations of 100 blobs
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

	let yOffset = 0;
	const blobAnimation = (isEvenFrame) => {
		//Find the offset between a blob's screen space Y position and the in-div Y position
		//The getBoundingClientRect calculation is expensive, so only calculate once every 2 frames
		if (isEvenFrame) {
			yOffset = blobLocations[0].element.getBoundingClientRect().top - blobLocations[0].y
		}

		//For each blob in bloblocations, find its distance to the cursor
		blobLocations.forEach((blob, index) => {
			//Get the center screen coordinate of each blob
			let blobX = blob.x + blobSize/2;
			let blobY = blob.y + yOffset + blobSize/2;

			let newX;
			let newY;

			//Calculate the distance to the cursor
			let distance = Math.sqrt(Math.pow(blobX - cursorX, 2) + Math.pow(blobY - cursorY, 2));
			//If the distance is less than 200px, move the blob away from the cursor
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
			//As long as the new coordinates are within the bounds of the container and significant enough, update the blob's location
			if (newX + blobSize/2 > 0 && newX - blobSize/2 < blobContainerWidth - blobSize && Math.abs(newX - blob.x) > 1.5) {
				blob.x = newX;
			}
			if (newY + blobSize/2 > 0 && newY - blobSize/2 < blobContainerHeight - blobSize && Math.abs(newY - blob.y) > 1.5) {
				blob.y = newY;
			}
		})
		//Reassignment so Svelte updates the blobs in the DOM.
		blobLocations = blobLocations;
		window.requestAnimationFrame(()=>{blobAnimation(!isEvenFrame)});
	};

	//Mobile detection code from slickscroll. Don't make blobs interactive if the client is mobile to increase
	//performance and also since there is no cursor to follow.
	function isMobile() {
		let check = false;
		(function (a) {
			if (/(android|bb\d+|meego).+mobile|avantgo|bada\/|blackberry|blazer|compal|elaine|fennec|hiptop|iemobile|ip(hone|od)|iris|kindle|lge |maemo|midp|mmp|mobile.+firefox|netfront|opera m(ob|in)i|palm( os)?|phone|p(ixi|re)\/|plucker|pocket|psp|series(4|6)0|symbian|treo|up\.(browser|link)|vodafone|wap|windows ce|xda|xiino/i.test(a) || /1207|6310|6590|3gso|4thp|50[1-6]i|770s|802s|a wa|abac|ac(er|oo|s\-)|ai(ko|rn)|al(av|ca|co)|amoi|an(ex|ny|yw)|aptu|ar(ch|go)|as(te|us)|attw|au(di|\-m|r |s )|avan|be(ck|ll|nq)|bi(lb|rd)|bl(ac|az)|br(e|v)w|bumb|bw\-(n|u)|c55\/|capi|ccwa|cdm\-|cell|chtm|cldc|cmd\-|co(mp|nd)|craw|da(it|ll|ng)|dbte|dc\-s|devi|dica|dmob|do(c|p)o|ds(12|\-d)|el(49|ai)|em(l2|ul)|er(ic|k0)|esl8|ez([4-7]0|os|wa|ze)|fetc|fly(\-|_)|g1 u|g560|gene|gf\-5|g\-mo|go(\.w|od)|gr(ad|un)|haie|hcit|hd\-(m|p|t)|hei\-|hi(pt|ta)|hp( i|ip)|hs\-c|ht(c(\-| |_|a|g|p|s|t)|tp)|hu(aw|tc)|i\-(20|go|ma)|i230|iac( |\-|\/)|ibro|idea|ig01|ikom|im1k|inno|ipaq|iris|ja(t|v)a|jbro|jemu|jigs|kddi|keji|kgt( |\/)|klon|kpt |kwc\-|kyo(c|k)|le(no|xi)|lg( g|\/(k|l|u)|50|54|\-[a-w])|libw|lynx|m1\-w|m3ga|m50\/|ma(te|ui|xo)|mc(01|21|ca)|m\-cr|me(rc|ri)|mi(o8|oa|ts)|mmef|mo(01|02|bi|de|do|t(\-| |o|v)|zz)|mt(50|p1|v )|mwbp|mywa|n10[0-2]|n20[2-3]|n30(0|2)|n50(0|2|5)|n7(0(0|1)|10)|ne((c|m)\-|on|tf|wf|wg|wt)|nok(6|i)|nzph|o2im|op(ti|wv)|oran|owg1|p800|pan(a|d|t)|pdxg|pg(13|\-([1-8]|c))|phil|pire|pl(ay|uc)|pn\-2|po(ck|rt|se)|prox|psio|pt\-g|qa\-a|qc(07|12|21|32|60|\-[2-7]|i\-)|qtek|r380|r600|raks|rim9|ro(ve|zo)|s55\/|sa(ge|ma|mm|ms|ny|va)|sc(01|h\-|oo|p\-)|sdk\/|se(c(\-|0|1)|47|mc|nd|ri)|sgh\-|shar|sie(\-|m)|sk\-0|sl(45|id)|sm(al|ar|b3|it|t5)|so(ft|ny)|sp(01|h\-|v\-|v )|sy(01|mb)|t2(18|50)|t6(00|10|18)|ta(gt|lk)|tcl\-|tdg\-|tel(i|m)|tim\-|t\-mo|to(pl|sh)|ts(70|m\-|m3|m5)|tx\-9|up(\.b|g1|si)|utst|v400|v750|veri|vi(rg|te)|vk(40|5[0-3]|\-v)|vm40|voda|vulc|vx(52|53|60|61|70|80|81|83|85|98)|w3c(\-| )|webc|whit|wi(g |nc|nw)|wmlb|wonu|x700|yas\-|your|zeto|zte\-/i.test(a.substring(0, 4)))
				check = true;
		}) (navigator.userAgent || navigator.vendor);

		if (!check && CSS.supports) check = !CSS.supports("position", "sticky");
		return check;
	}

	if (!isMobile()) {
		window.requestAnimationFrame(()=>{blobAnimation(true)});
	}

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
			<p>I'm a CS student who has been writing code for as long as I can remember, creating projects small and big.</p>
			<h1>A developer</h1>
		</div>
		<div class="card parallaxFast">
			<h1>Across Languages,</h1>
			<p>Over the years, I've learned Javascript, Python, C++, C#, and other related tech. I'm also fluent in English and Korean, and learning French.</p>
		</div>
		<div class="card parallaxFast">
			<h1>Over Borders,</h1>
			<p>I have lived all across the globe, making friends in Italy, Saudi Arabia, South Korea, and the US.</p>
		</div>
		<div class="card parallaxFast">
			<h1>& Fueled by Curiosity</h1>
			<p>I am always looking to grow and learn, fascinated every day by the possibilities in front of us every day.</p>
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
		background-color: var(--color-translucent);
		z-index: 10;
		padding: 1em;
		border-radius: 1em;
		width: 90%;
		justify-self: center;

		transition: scale 0.5s ease, box-shadow 0.5s ease;
		box-shadow: 0 0 10px 0 rgba(0,0,0,0.75);
	}
	.card:hover{
		scale: 1.008;
		box-shadow: 4px 4px 13px  rgba(0,0,0,0.75);
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
		background-color: var(--color-accent);
		position: absolute;
		border-radius: 50%;
		width: 250px;
		height: 250px;
		z-index: -50;
	}

	h1 {
		margin:0;
		font-size: 2em;
		line-height: 1.4em;
	}
	p {
		font-size: 1.2em;
		line-height: 1.44em;
	}
</style>