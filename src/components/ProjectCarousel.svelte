<script>
	import {onMount} from "svelte";
	import ProjectPage from "./ProjectPage.svelte";
	import Project1 from "./Projects/Project1.svelte";
	import Project2 from "./Projects/Project2.svelte";
	import Project3 from "./Projects/Project3.svelte";
	import Project4 from "./Projects/Project4.svelte";

	let carouselEnabled = true;
	let mouseDownAt = 0;
	let percentage = 0;
	export let nextPercentage = 0;
	let prevPercentage = 0;
	const normalWidth = 40;
	const gapWidth = 4;

	let track;

	let firstPageLocation;
	let lastPageLocation;

	const mousedown = e => {
		//If it is a touch event, get the first touch
		if (e.type === "touchstart") {
			e = e.touches[0];
		}

		mouseDownAt = e.clientX;
	}

	const mousemove = e => {
		if (mouseDownAt === 0 || !carouselEnabled) return;

		//If it is a touch event, get the first touch
		if (e.type === "touchmove") {
			e = e.touches[0];
		}

		const mouseDelta = mouseDownAt - e.clientX;
		const maxDelta = window.innerWidth / 1.5;

		percentage = (mouseDelta / maxDelta) * -100;
		nextPercentage = prevPercentage + percentage;
		nextPercentage = Math.min(0, Math.max(-100, nextPercentage));

		//Normalize the percentage to the firstPageLocation and lastPageLocation
		let normPercentage = ((nextPercentage + 100)/(0 + 100)) * (firstPageLocation - lastPageLocation) + lastPageLocation;

		track.animate({
			transform: `translate(${normPercentage}%, -50%)`
		}, {duration: 1200, fill: "forwards"});
		Array.from(track.children).forEach(child => {
			//Select the image element and animate.
			child.children[0].children[0].animate({
				objectPosition: `${normPercentage + 100}% center`
			}, {duration: 1200, fill: "forwards"});
		});
	}

	const mouseup = () => {
		mouseDownAt = 0;
		prevPercentage = nextPercentage;
	}

	//Scrolls the carousel so that the selected project is centered.
	const scrollToPage = (index, mode = null) => {
		//Toggles the carousel
		if (carouselEnabled && mode === null){
			carouselEnabled = false;
			mode ??= "expand";
		} else {
			carouselEnabled = true;
			mode ??= "shrink";
		}

		//Calculates the width of the expanded project page in vmin.
		let bigElemWidth;
		if (window.innerWidth > window.innerHeight){
			bigElemWidth = 100 * (window.innerWidth/window.innerHeight);
		} else {
			bigElemWidth = 100;
		}

		//Calculates the percentage of the carousel that needs to be scrolled.
		let totalWidth;
		let targetX;
		//If mode is "expand", use big elemWidth, if not, use normalWidth
		if (mode === "expand") {
			totalWidth = (track.children.length - 1) * (normalWidth + gapWidth) + bigElemWidth;
			targetX = index * (normalWidth + gapWidth) + bigElemWidth / 2;
		}else {
			totalWidth = (track.children.length - 1) * (normalWidth + gapWidth) + normalWidth;
			targetX = index * (normalWidth + gapWidth) + normalWidth / 2;
		}
		//This will map output the offset that the carousel needs to be scrolled to.
		let normPercentage = targetX / totalWidth * -100;
		//Un-Normalize the percentage so that it maps from 0 to -100
		nextPercentage = (normPercentage - lastPageLocation) / (firstPageLocation - lastPageLocation) * 100 - 100;
		prevPercentage = nextPercentage;

		//Animate the Carousel into place
		track.animate({
			transform: `translate(${normPercentage}%, -50%)`
		}, {duration: 1000, fill: "forwards", easing: "ease-in-out"});
		Array.from(track.children).forEach(child => {
			//Select the image element and animate.
			child.children[0].children[0].animate({
				objectPosition: `${normPercentage + 100}% center`
			}, {duration: 1000, fill: "forwards", easing: "ease-in-out"});
		});
	}

	onMount(() => {
		firstPageLocation = (normalWidth / 2) / ((track.children.length - 1) * (normalWidth + gapWidth) + normalWidth) * -100;
		lastPageLocation = ((track.children.length -1 ) * (normalWidth + gapWidth) + normalWidth / 2) / ((track.children.length - 1) * (normalWidth + gapWidth) + normalWidth) * -100;

		scrollToPage(0, "shrink");
	})
</script>

<h1>
	Projects
</h1>
<div class="wrapper"
	 on:mousedown={mousedown}
	 on:mousemove={mousemove}
	 on:mouseup={mouseup}
	 on:touchstart={mousedown}
	 on:touchmove={mousemove}
	 on:touchend={mouseup}
	 style="--imageOffset: {nextPercentage + 100}%;">

	<div id="image-track" bind:this={track}>
		<ProjectPage on:confirmedClick={()=>{scrollToPage(0)}}>
			<img class="image" src="/images/1_Book.png" draggable="false" alt=""/>
			<span slot="content"><Project1/></span>
		</ProjectPage>
		<ProjectPage on:confirmedClick={()=>{scrollToPage(1)}}>
			<img class="image" src="/images/2_OmoriSpace.png" draggable="false" alt=""/>
			<span slot="content"><Project2/></span>
		</ProjectPage>
		<ProjectPage on:confirmedClick={()=>{scrollToPage(2)}}>
			<img class="image" src="/images/3_Key.png" draggable="false" alt=""/>
			<span slot="content"><Project3/></span>
		</ProjectPage>
		<ProjectPage on:confirmedClick={()=>{scrollToPage(3)}}>
			<img class="image" src="/images/4_Room.png" draggable="false" alt=""/>
			<span slot="content"><Project4/></span>
		</ProjectPage>
	</div>
</div>

<style>
	.wrapper {
		position: absolute;
		width: 100%;
		height: 100%;
		margin: 0;
		padding: 0;
		overflow: hidden;
	}
	h1{
		font-size: 7vmin;
		color: #EDF5FF;
		margin-top: 7vmin;
		margin-left: 7vmin;
		margin-bottom: -14vmin;
	}

	.image {
		border-radius: 8px;
		width: 40vmin;
		height: 56vmin;
		object-fit: cover;
		object-position: 100% center;
		opacity: 0.7;
	}

	#image-track {
		display: flex;
		gap: 4vmin;
		position: absolute;
		left: 50%;
		top: 50%;
		transform: translate(0%, -50%);
		align-items: center;
		z-index: 100;
	}
</style>