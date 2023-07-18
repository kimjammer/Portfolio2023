<script>
	import {onMount} from "svelte";

	let imageWrapper;
	let image;

	onMount(()=>{
		image = imageWrapper.children[0];
	});
</script>

<div class="wrapper" on:click>
	<span bind:this={imageWrapper}><slot></slot></span>
	<div class="overlay">
		<h2><slot name="title"></slot></h2>
		<p><slot name="description"></slot></p>
	</div>
</div>

<style>
	.wrapper {
		user-select:none;
		display: flex;
		filter: blur(0px);
	}
	.overlay {
		position: fixed;
		bottom: 0;
		left: 0;
		padding: 1em;
		background-color: #EDF5FF7F;
		border-radius: 0.5em;
		margin: 1em;
		opacity: 0;
		transition: 0.5s ease;
	}
	.wrapper:hover .overlay {
		opacity: 1;
	}
</style>