<script>
	import {createEventDispatcher, onMount} from "svelte";
	export let selected = false;

	let imageWrapper;
	let content;
	let image;

	onMount(()=>{
		image = imageWrapper.children[0];
	});

	const dispatch = createEventDispatcher();
	let startX = 0;
	//Keep track of where the click/touch started
	const handleStart = (event) => {
		if (event.type === "touchstart") {
			event = event.touches[0];
		}
		startX = event.clientX;
	}
	const handleEnd = (event) => {
		if (event.type === "touchstart") {
			event = event.touches[0];
		}
		if (!selected && Math.abs(event.clientX - startX) < 10) {
			expand();
			dispatch('confirmedClick');
		}else if (selected && Math.abs(event.clientX - startX) < 10) {
			shrink();
			dispatch('confirmedClick');
		}
	}

	const expand = () => {
		selected = true;
		image.animate({
			width: `100vw`,
			height: `100vh`,
			borderRadius: `0px`
		}, {duration: 1200, easing: "ease-in-out", fill: "both"});
		content.animate({
			width: `100vw`,
			height: `100vh`,
			opacity: `100%`
		}, {duration: 1200, easing: "ease-in-out", fill: "both"});
	}

	const shrink = () => {
		selected = false;
		image.animate({
			width: `40vmin`,
			height: `56vmin`,
			borderRadius: `8px`,
			objectPosition: `center center`
		}, {duration: 1200, easing: "ease-in-out", fill: "both"});
		content.animate({
			width: `40vmin`,
			height: `56vmin`,
			opacity: `0%`
		}, {duration: 1200, easing: "ease-in-out", fill: "both"});
	}
</script>

<!--<div class="wrapper" on:click={handleClick} on:keypress={handleClick} on:click>-->
<div class="wrapper" on:mousedown={handleStart} on:mouseup={handleEnd} on:touchstart={handleStart} on:touchend={handleEnd} on:click>
	<span bind:this={imageWrapper}><slot></slot></span>
	<div bind:this={content} class="content"><slot name="content"></slot></div>
</div>

<style>
	.wrapper {
		user-select:none;
		display: flex;
	}
	.content {
		position: absolute;
		width: 40vmin;
		height: 56vmin;
		opacity: 0;
	}
</style>