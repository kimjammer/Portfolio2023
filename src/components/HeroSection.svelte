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
				<img src="/icons/Logo.svg" alt="KimJammer Logo">
			</div>

			<p>Developer. Student. Designer.</p>

			<div class="contacts">
				<ContactChip link="https://github.com/kimjammer">
					<img class="contactLogo" src="/icons/github.svg" alt="Github">
				</ContactChip>
				<ContactChip link="https://discord.com/users/424546246980665344">
					<img class="contactLogo" src="/icons/discord.svg" alt="Discord">
				</ContactChip>
				<ContactChip link="https://twitter.com/kimjammer1">
					<img class="contactLogo" src="/icons/twitter.svg" alt="Twitter">
				</ContactChip>
				<ContactChip link="https://mastodon.social/@kimjammer">
					<img class="contactLogo" src="/icons/mastodon.svg" alt="Mastodon">
				</ContactChip>
				<ContactChip link="mailto:contact@kimjammer.com">
					<img class="contactLogo" src="/icons/email.svg" alt="Email">
				</ContactChip>
			</div>


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
	.card{
		position: relative;
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
		box-shadow: 0 0 10px 0 #596be3;
	}
	.card:hover{
		scale: 1.015;
		box-shadow: 4px 4px 13px 0 #596be3;
	}
	@media (max-height: 890px) {
		.card {
			height: 70vh;
		}
	}
	@keyframes fadeIn {
		0% {
			opacity: 0;
		}
		100% {
			opacity: 1;
		}
	}

	.contacts{
		display: flex;
		flex-wrap: wrap;
		justify-content: start;
		gap: 1em;
		align-items: center;
		position: absolute;
		bottom: 2em;
		margin-right: 2em;
	}
	@media (min-width:430px) {
		.contacts{
			gap: 1.5em;
		}
	}
	.contactLogo {
		width: 70%;
		position: relative;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
	}

	.header{
		display: flex;
		justify-content: space-between;
		align-items: flex-start;
		flex-direction: column-reverse;
		gap: 1em;
	}
	@media (min-width:430px) {
		.header {
			flex-direction: row;
		}
	}

	img{
		width: 20vmin;
	}
	h1 {
		margin: 0;
		font-size: 7vmin;
		line-height: 8.4vmin;
		font-weight: 700;
	}
	p {
		margin: 0;
		font-size: 5vmin;
		line-height: 7.5vmin;
	}
</style>