<script lang="ts">
	import { browser } from '$app/env';

	let loadingWords = true;
	let loadedCount = 0;
	const languages = ['en', 'nl'];
	let activeLanguages = [...languages];
	const languageWords = {};
	let length = 8;
	let knownLetters = [];
	let allWords = [];
	let letterCount = {};

	if (browser) {
		for (const lang of languages) {
			fetch(`/${lang}.txt`)
				.then((d) => d.text())
				.then((d) => {
					languageWords[lang] = d.split('\n');
					loadedCount++;
					if (loadedCount === languages.length) {
						loadingWords = false;
						updateWords();
					}
				});
		}
	}

	function toggleLanguage(lang: string) {
		console.log(lang);
		if (activeLanguages.includes(lang)) {
			activeLanguages = activeLanguages.filter((l) => l !== lang);
		} else {
			activeLanguages = [...activeLanguages, lang];
		}
		updateWords();
	}

	function updateWords() {
		letterCount = {};
		for (let i = 0; i < length; i++) {
			knownLetters[i] = knownLetters[i] || '';
		}

		allWords = [];
		for (let lang of activeLanguages) {
			allWords = [...allWords, ...languageWords[lang]];
		}
		allWords = allWords.filter(Boolean).sort((a, b) => a.localeCompare(b));

		// Filtering
		allWords = allWords
			.filter((v) => v.trim().length === length)
			.filter((word) => {
				for (let i = 0; i < length; i++) {
					let l = word[i].toLowerCase();
					if ((knownLetters[i] || '').trim() !== '') {
						if ((word[i] || '').toLowerCase() !== (knownLetters[i] || '').toLowerCase())
							return false;
					}
				}

				return true;
			});

		for (let word of allWords) {
			for (let i = 0; i < length; i++) {
				let l = word[i].toLowerCase();
				letterCount[l] = letterCount[l] + 1 || 1;
			}
		}
		return allWords;
	}
</script>

<h1>Galgje cheats</h1>

<p>
	Welkom op <a href="https://galgjecheats.nl">galgjecheats.nl</a>. Wij zorgen dat je eindelijk kan
	winnen door je de beste letters te laten zien, en ook mogelijke woorden.
</p>

<hr />

<div class="language-selector">
	Kies je talen:
	<div>
		<span
			class="link"
			class:active={activeLanguages.includes('nl')}
			on:click={() => toggleLanguage('nl')}>Nederlands</span
		>
		<span
			class="link"
			class:active={activeLanguages.includes('en')}
			on:click={() => toggleLanguage('en')}>Engels</span
		>
	</div>
</div>

<div>
	<p>lengte woord</p>
	<input type="range" min="1" max="20" bind:value={length} on:change={updateWords} />
	{length}
</div>

<div class="layout">
	<div>
		{#each Array(length)
			.fill(0)
			.map((_, i) => i) as i}
			<span class="letter" class:active={knownLetters.includes(i)}>
				<div class="num">
					{length >= 10 ? (i + 1).toString().padStart(2, '0') : i + 1}
					<input
						type="text"
						bind:value={knownLetters[i]}
						on:input={(e) => {
							// @ts-ignore
							e.currentTarget.value = (e.data || '').slice(0, 1).toLowerCase();

							// @ts-ignore
							let wrapper = e.target.parentNode.parentNode;
							const allChildren = wrapper.parentNode.children;
							const i = Array.from(allChildren).indexOf(wrapper);
							if (allChildren[i + 1]) {
								allChildren[i + 1].querySelector('input').focus();
							}
							updateWords();
						}}
					/>
				</div>
			</span>
		{/each}
	</div>
	{#if !loadingWords}
		<div>
			<div>
				Beste letter keuzes: {Object.entries(letterCount)
					// @ts-ignore
					.sort((a, b) => b[1] - a[1])
					.map((v) => v[0].toUpperCase())
					.filter((v) => !knownLetters.find((t) => t.toLowerCase() === v.toLowerCase()))
					.slice(0, 10)
					.join(', ') || (allWords.length === 0 ? 'idk man je bent gwn fucked' : 'YAY')}
			</div>
			<div>
				{Math.min(allWords.length, 300)} woorden:
				<div class="words">
					{#each allWords.slice(0, 300) as word}
						<a
							class="word"
							href={`https://www.google.com/search?q=${
								word.slice(0, 1).toUpperCase() + word.slice(1)
							}`}
							target="_blank">{word.slice(0, 1).toUpperCase() + word.slice(1)}</a
						>
					{/each}
				</div>
			</div>
		</div>
	{/if}
</div>

<style>
	.num {
		font-variant-numeric: tabular-nums;
		display: flex;
		align-items: center;
	}
	.num input {
		width: 2rem;
		font-size: 3rem;
		text-align: center;
		margin-left: 1rem;
	}
	.layout {
		display: grid;
		grid-template-columns: auto 1fr;
		grid-gap: 30px;
	}
	.words {
		display: grid;
		grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));
		grid-gap: 2px;
	}
	.words p {
		margin: 0;
	}
</style>
