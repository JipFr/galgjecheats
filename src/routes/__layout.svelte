<svelte:head>
	<title>galgjecheats.nl</title>
	<meta name="og:title" content="galgjecheats.nl" />
	<meta name="twitter:title" content="galgjecheats.nl" />
	<meta
		name="description"
		content="Wil jij eindelijk niet langer de loser van de klas zijn? Klik dan op deze link!"
	/>
	<meta
		name="og:description"
		content="Wil jij eindelijk niet langer de loser van de klas zijn? Klik dan op deze link!"
	/>
	<meta
		name="twitter:description"
		content="Wil jij eindelijk niet langer de loser van de klas zijn? Klik dan op deze link!"
	/>
	<meta name="image" content="https://i.imgur.com/UFdtpmS.png" />
	<meta name="og:image" content="https://i.imgur.com/UFdtpmS.png" />
	<meta name="twitter:image:src" content="https://i.imgur.com/UFdtpmS.png" />
</svelte:head>

<main>
	<slot />
</main>

<style>
	:global(body) {
		background: black;
		color: lime;
	}
	main {
		width: 90%;
		max-width: 600px;
		margin: 80px auto;
		font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell,
			'Open Sans', 'Helvetica Neue', sans-serif;
	}
	:global(a, .link) {
		background-image: linear-gradient(to left, violet, indigo, blue, green, yellow, orange, red);
	}
	:global(a.active, .link.active) {
		color: white;
	}
	:global(a:not(.active), .link:not(.active)) {
		-webkit-background-clip: text;
		background-clip: text;
		-webkit-text-fill-color: transparent;
	}
	:global(.link) {
		display: inline-block;
		padding: 10px;
		cursor: pointer;
	}
	:global(.link:hover) {
		animation: spin 1s infinite linear;
	}
	@keyframes spin {
		from {
			transform: rotate(0deg);
		}
		to {
			transform: rotate(360deg);
		}
	}
</style>
