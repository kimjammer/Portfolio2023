<script>
	import { browser } from '$app/environment';
	import slickScroll from '../../node_modules/slickscrolljs/dist/slickscroll.es.min.js';
	import IntroAnimation from '../components/IntroAnimation.svelte';
	import Background from "../components/Background.svelte";
	import HeroSection from "../components/HeroSection.svelte";
	import StorySection from "../components/StorySection.svelte";
	import ProjectCarousel from "../components/ProjectCarousel.svelte";
	import {navigating} from "$app/stores";
	import {onMount} from "svelte";

	let scrollable;
	//Wait 500ms then make introAnimationDone true (The curtain will have come down.)
	let introAnimationDone = false;
	setTimeout(() => {
		introAnimationDone = true;
	}, 500);

	//Initialize slickScroll once we're ready
	setTimeout(() => {
		if (!browser) return;
		let slick = new slickScroll({
			root: "#scrollable",
			offsets: [
				{
					element: ".parallaxFast",
					speedY: 0.9 // Vertical speed
				},
				{
					element: ".parallaxMedium",
					speedY: 0.8 // Vertical speed
				},
				{
					element: ".parallaxSlow",
					speedY: 0.7 // Vertical speed
				}
			]
		});
		scrollable.style.overflowX = "hidden";
	}, 500);
</script>

<IntroAnimation/>
<!--Only show if the intro animation is done playing-->
{#if introAnimationDone}
	<Background/>

	<div id="scrollable" bind:this={scrollable}>
		<HeroSection/>
		<StorySection/>
		<ProjectCarousel/>
	</div>
{/if}

<style>
	:global(body) {
		margin: 0;
		padding: 0;
		overflow-x: hidden;
		scrollbar-width: none; /* Firefox */
		-ms-overflow-style: none; /* Internet Explorer 10+ */
	}
	:global(body)::-webkit-scrollbar { /* WebKit */
		width: 0;
		height: 0;
	}
	:global(h1) {
		font-family: 'Manrope', sans-serif;
	}
	:global(p) {
		font-family: 'Manrope', sans-serif;
	}
	#scrollable {
		height: 100vh;
		width: 100vw;

		scrollbar-width: none; /* Firefox */
		-ms-overflow-style: none; /* Internet Explorer 10+ */
	}
	#scrollable::-webkit-scrollbar { /* WebKit */
		width: 0;
		height: 0;
	}

	/*:global(*) {*/
	/*	outline: 1px solid #f00 !important;*/
	/*}*/
</style>