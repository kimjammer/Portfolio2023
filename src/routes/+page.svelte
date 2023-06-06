<script>
  import IntroAnimation from '../components/IntroAnimation.svelte';
  import Background from "../components/Background.svelte";
  import HeroSection from "../components/HeroSection.svelte";
  import StorySection from "../components/StorySection.svelte";

  //Wait 300ms then make introAnimationDone true (The curtain will have come down.)
  let introAnimationDone = false;
  setTimeout(() => {
	introAnimationDone = true;
  }, 500);

</script>

<IntroAnimation/>

<!--Only show if the intro animation is done playing-->
{#if introAnimationDone}

	<Background/>

	<HeroSection/>
	<StorySection/>


{/if}

<style>
	:global(body) {
		margin: 0;
		padding: 0;
	}
</style>