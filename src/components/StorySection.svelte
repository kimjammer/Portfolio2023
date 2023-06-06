<script>
	import {onMount} from "svelte";
	//Make the metaball div match the height of the content div
	let metaballBox;
	let metaballContainer;
	let backgroundbox;
	let contentBox;
	let metaballLocations = [];


	onMount(async ()=> {
		$: metaballBox.style.height = `${contentBox.clientHeight}px`;
		$: backgroundbox.style.height = `${contentBox.clientHeight}px`;

		//Make a list containing the random locations of 200 metaballs
		for (let i = 0; i < 200; i++) {
			metaballLocations.push({
				x: `${Math.random() * metaballBox.clientWidth - 200}px`,
				y: `${Math.random() * metaballBox.clientHeight - 200}px`
			})
		}
		metaballLocations = metaballLocations;
	})
</script>

<div class="container">
	<!--This div houses the interactive metaballs int he background	-->
	<div class="background" bind:this={backgroundbox}>
	</div>
		<div class="recolor">
			<div class="metasharpening" bind:this={metaballBox}>
				<div class="metablurring" bind:this={metaballContainer}>
					<!--The metaballs	-->
					{#each metaballLocations as location}
						<div class="ball" style="top:{location.y}; left:{location.x}"></div>
					{/each}
				</div>
			</div>
		</div>



	<div class="content" bind:this={contentBox}>
		<div class="card">
			<h1>Story part 1</h1>
			<p>Some body text about some part of my life that is hopefully interesting or something i dont know lol.</p>
		</div>
		<div class="card">
			<h1>Story part 1</h1>
			<p>Some body text about some part of my life that is hopefully interesting or something i dont know lol.</p>
		</div>
		<div class="card">
			<h1>Story part 1</h1>
			<p>Some body text about some part of my life that is hopefully interesting or something i dont know lol.</p>
		</div>
	</div>
</div>

<style lang="scss">
	.content {
		border: red solid 1px;
		display: flex;
		flex-direction: column;
		gap: 2em;
		box-sizing: border-box;
	}

	.metasharpening {
		filter: contrast(10);
		width:100%;
		position: absolute;
		z-index: -1;
		border: green solid 1px;
		box-sizing: border-box;
		overflow-x: clip;
		overflow-y: visible;
		background-color: #000000;
	}
	.metablurring {
		filter: blur(15px);
		width:100%;
		height:100%;
	}
	.recolor{
		mix-blend-mode: multiply;
	}
	.background {
		background-color: #b2ff07;
		position:absolute;
		width: 100%;

		border: blue solid 5px;
		z-index:-10;
	}
	.ball{
		background-color: #ffffff;
		position: absolute;
		border-radius: 50%;
		width: 170px;
		height: 170px;
	}
</style>