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
	<div class="skillChips">
		<div class="chip"><slot name="chip2"></slot></div>
		<div class="chip"><slot name="chip1"></slot></div>
	</div>
</div>

<style>
	.wrapper {
		user-select:none;
		display: flex;
		transform: translate(0,0);
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
		backdrop-filter: blur(5px);
	}
	.wrapper:hover .overlay {
		opacity: 1;
	}
	.skillChips {
		position: fixed;
		display: flex;
		bottom: 0;
		right: 1em;
		gap: 0.5em;
		transition: 0.3s ease;
	}
	.wrapper:hover .skillChips{
		opacity: 0;
	}
	.chip {
		margin: 1em 0 1em 0;
		width: 2.1em;
		height: 2.1em;
	}
	h2 {
		margin: 0;
	}
	p {
		margin: 0;
	}
</style>