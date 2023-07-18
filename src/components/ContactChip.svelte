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
		width: 2em;
		height: 2em;
		background-color: #EDF5FF7F;
		border: #596be3 1px solid;
		transition: 0.5s ease;
	}
	@media (min-width:430px) {
		.container {
			width: 3em;
			height: 3em;
		}
	}
	.container:hover {
		box-shadow: 4px 4px 13px  rgba(0,0,0,0.75);
	}
</style>