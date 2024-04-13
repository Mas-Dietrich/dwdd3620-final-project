<script>
	import Fa from 'svelte-fa'
	import {faCheck, faX} from '@fortawesome/free-solid-svg-icons'

	let initialSettings = true

	let lastName = '';

	let filterSettings = false;

	let babyNameType = '';

	let popular = false;

	let selectionMade = false;

	let babyNames = [];

	let apiKey = '2LP0z26UuHCOic1ZWZ/x5w==7WiMg5Quumm85tl6';

	let showCards = false

	async function getBabyNames() {
		try {
			const headers = new Headers();
			headers.append('X-API-Key', apiKey)

			let apiUrl = 'https://api.api-ninjas.com/v1/babynames'
			if(babyNameType) {
				apiUrl += `?gender=${babyNameType}`
			}
			if(popular) {
				apiUrl += `&popular_only=true`
			}
		const response = await fetch(apiUrl, {
			method: 'GET',
			headers: headers
		})
		const data = await response.json()
		babyNames = data
		console.log(babyNames)
		displayBabyNames()

	} catch(error) {
		console.error('Error fetching baby names:', error.message)
	}
}


	function setFilterSettings() {
		filterSettings = true;
		initialSettings = false
	}

	function isSelectionMade() {
		selectionMade = babyNameType !== '';
	}

	function displayBabyNames() {
		filterSettings = false
		showCards = true
	}

</script>

<div class="flex flex-col justify-center h-full items-center p-12">
	<div class="card w-3/4 p-4 space-y-5 text-center my-auto">
		<h1 class="h1 hover:animate-pulse">BabyBabble</h1>
		<p class="italic">Swipe names for the big day!</p>

		{#if initialSettings}
			<h2 class="text-2xl font-semibold">Let's start with your name (if you want)</h2>
			<div>
				<p>Your last name is: {lastName}</p>
				<input class='input w-1/2 my-4' bind:value={lastName} />
			</div>
			<button class="btn variant-filled hover:animate-pulse" on:click={setFilterSettings}>Continue</button>
		{/if}
		{#if filterSettings}
			<label class="text-2xl font-semibold" for="babyNameType">What kind of names are you looking for?</label>
			<select 
				class='select w-1/2'
				size='3'
				name="babyNameType"
				id="babyNameType"
				placeholder="Select name type"
				bind:value={babyNameType}
				on:change={isSelectionMade}
				required
			>
				<option value="neutral">Any</option>
				<option value="girl">Girl Names</option>
				<option value="boy">Boy Names</option>
			</select>

			<label for="popularNames" class="flex justify-center">
				<input class="checkbox mr-2" type="checkbox" bind:checked={popular} />
				<p>Search by Popular Names?</p>
			</label>

			<button class="btn variant-soft-secondary hover:animate-pulse" disabled={!selectionMade} on:click={getBabyNames}
				>Let's get some names!</button
			>
		{/if}
	</div>
	<div class="flex justify-center flex-col w-3/4">
		{#if displayBabyNames}
			{#each babyNames as babyName}
				<div class="card p-8 hover:animate-pulse h-96 my-8 relative">
					<h2 class="text-3xl font-bold text-center">{babyName} {lastName}</h2>
					<Fa class="absolute left-5 bottom-[50%] text-red-600 text-xl" icon={faX} />
					<Fa class="absolute bottom-[50%] right-5 text-green-600 text-xl" icon={faCheck} />
				</div>
			{/each}
		{/if}
	</div>
</div>
