<script>
	export let link;
</script>

<a href={link}>
	<div class="container">
		<slot class="image"></slot>
	</div>
</a>

<style>
	.container {
		border-radius: 0.5em;
		width: 3em;
		height: 3em;
		background-color: #EDF5FF7F;
		border: #596be3 1px solid;
	}
</style>