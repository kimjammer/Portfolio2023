<script>
	import {onMount} from "svelte";
	import ProjectPage from "./ProjectPage.svelte";

	let carouselEnabled = true;
	let mouseDownAt = 0;
	let percentage = 0;
	export let nextPercentage = 0;
	let prevPercentage = 0;
	let prevNormPercentage;
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

		//If the track is currently being animated, animate to next pos, otherwise, animate from last finished pos
		if (track.getAnimations().length === 0) {
			track.animate(
					[
						{transform: `translate(${prevNormPercentage}%, -50%)`},
						{transform: `translate(${normPercentage}%, -50%)`}
					],
					{duration: 1200, fill: "forwards"});
		}else {
			track.animate({transform: `translate(${normPercentage}%, -50%)`}, {duration: 1200, fill: "forwards"});
		}
		Array.from(track.children).forEach(child => {
			//Select the image element and animate.
			child.children[0].children[0].animate({
				objectPosition: `${normPercentage + 100}% center`
			}, {duration: 1200, fill: "forwards"});
		});
		prevNormPercentage = normPercentage;
	}

	const mouseup = () => {
		mouseDownAt = 0;
		prevPercentage = nextPercentage;
	}

	onMount(() => {
		firstPageLocation = (normalWidth / 2) / ((track.children.length - 1) * (normalWidth + gapWidth) + normalWidth) * -100;
		lastPageLocation = ((track.children.length -1 ) * (normalWidth + gapWidth) + normalWidth / 2) / ((track.children.length - 1) * (normalWidth + gapWidth) + normalWidth) * -100;
		prevNormPercentage = (firstPageLocation - lastPageLocation) + lastPageLocation;

		track.animate({transform: `translate(${prevNormPercentage}%, -50%)`}, {duration: 1200, fill: "forwards"});
		Array.from(track.children).forEach(child => {
			//Select the image element and animate.
			child.children[0].children[0].animate({
				objectPosition: `${prevNormPercentage + 100}% center`
			}, {duration: 1200, fill: "forwards"});
		});
	});

	const handleScroll = (e) => {
		const mouseDelta = 50 * Math.sign(e.deltaY);
		const maxDelta = window.innerWidth / 1.5;

		percentage = (mouseDelta / maxDelta) * -100;
		nextPercentage = prevPercentage + percentage;
		nextPercentage = Math.min(0, Math.max(-100, nextPercentage));

		//Normalize the percentage to the firstPageLocation and lastPageLocation
		let normPercentage = ((nextPercentage + 100)/(0 + 100)) * (firstPageLocation - lastPageLocation) + lastPageLocation;

		//If the track is currently being animated, animate to next pos, otherwise, animate from last finished pos
		if (track.getAnimations().length === 0) {
			track.animate(
				[
					{transform: `translate(${prevNormPercentage}%, -50%)`},
					{transform: `translate(${normPercentage}%, -50%)`}
				],
				{duration: 1200, fill: "forwards"});
		}else {
			track.animate({transform: `translate(${normPercentage}%, -50%)`}, {duration: 1200, fill: "forwards"});
		}
		Array.from(track.children).forEach(child => {
			//Select the image element and animate.
			child.children[0].children[0].animate({
				objectPosition: `${normPercentage + 100}% center`
			}, {duration: 1200, fill: "forwards"});
		});

		prevPercentage = nextPercentage;
		prevNormPercentage = normPercentage;
	}
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
	 on:wheel={handleScroll}
	 style="--imageOffset: {nextPercentage + 100}%;">

	<div id="image-track" bind:this={track}>
		<ProjectPage >
			<img class="image" src="/images/1_Book.png" draggable="false" alt=""/>
			<span slot="title">Title 1</span>
			<span slot="description">Description 1</span>
		</ProjectPage>
		<ProjectPage >
			<img class="image" src="/images/2_OmoriSpace.png" draggable="false" alt=""/>
			<span slot="title">Title 2</span>
			<span slot="description">Description 2</span>
		</ProjectPage>
		<ProjectPage >
			<img class="image" src="/images/3_Key.png" draggable="false" alt=""/>
		</ProjectPage>
		<ProjectPage >
			<img class="image" src="/images/4_Room.png" draggable="false" alt=""/>
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
		width: 60vmin;
		height: 84vmin;
		object-fit: cover;
		object-position: 100% center;
		opacity: 0.7;
	}
	@media (min-width:430px) {
		.image {
			border-radius: 8px;
			width: 40vmin;
			height: 56vmin;
			object-fit: cover;
			object-position: 100% center;
			opacity: 0.7;
		}
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