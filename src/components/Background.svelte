<script>
	let cursorball;

	const mouseMoveHandler = (event) => {
		cursorball.animate({
			left: `${event.clientX}px`,
			top: `${event.clientY}px`
		}, {
			duration: 1500,
			fill: 'forwards',
			easing: 'ease'
		});
	}
</script>

<svelte:window on:mousemove={mouseMoveHandler}/>

<div class="container" >

</div>
<div class="weakBlurring"></div>
<div class="strongBlurring"></div>
<!--The dynamic metaball that follows the cursor	-->
<div class="cursorball" bind:this={cursorball}></div>


<style lang="scss">
	.container {
		position: absolute;
		left: 0;
		top: 0;
		width:100%;
		background-color: #05081D;
		z-index: -100;

		height: 5000px;
		overflow: hidden;
	}

	.strongBlurring{
		position: fixed;
		width: 100vw;
		height: 100%;
		backdrop-filter: blur(40px);
		z-index: -40;
	}

	.weakBlurring{
		 position: fixed;
		 width: 100vw;
		 height: 100%;
		 backdrop-filter: blur(30px);
		 z-index: -30;
	 }

	.cursorball{
	  position: fixed;
	  background: linear-gradient(
	  	to right,
	  	#596be3,
	  	#ffa07d
	  );
	  aspect-ratio: 1.1;
	  height: 20vmin;
	  left: 50%;
	  top: 50%;
	  translate: -50% -50%;
	  border-radius: 50%;
	  animation: idleWobble 10s infinite forwards;
	  z-index: -50;
	  overflow: hidden;
	}
	@keyframes idleWobble{
	  from{
		rotate: 0deg;
	  }
	  50%{
		aspect-ratio: 1.4;
		scale: 1.3;
	  }
	  to{
		rotate: 360deg;
	  }
	}

</style>