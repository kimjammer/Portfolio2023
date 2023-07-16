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
</div>

<style>
	.wrapper {
		user-select:none;
		display: flex;
	}
</style>