<script>
  import IntroAnimation from '../components/IntroAnimation.svelte';

  //Wait one second then make introAnimationDone true (The curtain will have come down.)
  let introAnimationDone = false;
  setTimeout(() => {
	introAnimationDone = true;
  }, 1000);

</script>

<IntroAnimation/>

<!--Only show if the intro animation is done playing-->
{#if introAnimationDone}


{/if}

<style>
	:global(body) {
		margin: 0;
		padding: 0;
	}
</style>