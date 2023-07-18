<script>

import ContactChip from "./ContactChip.svelte";
</script>

<div class="container">
	<div class="card parallaxMedium">
		<div class="content">
			<div class="header">
				<div>
					<h1>KimJammer</h1>
					<h1>John Kim</h1>
				</div>
				<img src="/Logo.svg" alt="KimJammer Logo">
			</div>

			<div class="contacts">
				<ContactChip link="https://github.com/kimjammer">
					<img class="contactLogo" src="/github.svg" alt="Github">
				</ContactChip>
				<ContactChip link="https://discord.com/users/424546246980665344">
					<img class="contactLogo" src="/discord.svg" alt="Discord">
				</ContactChip>
				<ContactChip link="https://twitter.com/kimjammer1">
					<img class="contactLogo" src="/twitter.svg" alt="Twitter">
				</ContactChip>
				<ContactChip link="https://mastodon.social/@kimjammer">
					<img class="contactLogo" src="/mastodon.svg" alt="Mastodon">
				</ContactChip>
				<ContactChip link="mailto:contact@kimjammer.com">
					<img class="contactLogo" src="/email.svg" alt="Email">
				</ContactChip>
			</div>

			<p>Developer. Student. Designer.</p>
		</div>
	</div>
</div>


<style lang="scss">
	.container {
		display: flex;
		justify-content: center;
		align-items: center;
		width: 100%;
		height: 100vh;
	}
	.contacts{
		display: flex;
		flex-wrap: wrap;
		justify-content: start;
		gap: 1em;
		align-items: center;
	}
	.contactLogo {
		width: 70%;
		position: relative;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
	}
	.card{
		width: 70vw;
		height: 50vh;
		background-color: #EDF5FF7F;
		z-index: 100;
		box-sizing: border-box;
		padding: 2em;
		border-radius: 2em;
		border: #596be3 1px solid;

		opacity: 0;
		animation: fadeIn 1s ease-in-out forwards;

		transition: 0.5s ease;
		box-shadow: 0px 0px 10px 0px #596be3;
	}
	.card:hover{
		scale: 1.015;
		box-shadow: 4px 4px 13px 0px #596be3;
	}
	@keyframes fadeIn {
		0% {
			opacity: 0;
		}
		100% {
			opacity: 1;
		}
	}

	.header{
		display: flex;
		justify-content: space-between;
		align-items: flex-start;
	}
	img{
		width: 20vmin;
	}
	h1 {
		font-size: 7vmin;
		margin-top: 0.2em;
	}
	p {
		font-size: 4vmin;
	}
</style>