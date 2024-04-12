<script>
	let initialSettings = true

	let lastName = '';

	let filterSettings = false;

	let babyNameType = '';

	let popular = false;

	let selectionMade = false;

	let babyNames = [];

	const apiKey = '2LP0z26UuHCOic1ZWZ/x5w==7WiMg5Quumm85tl6';

	async function getBabyNames() {
		try {
			const headers = new Headers();
			headers.append('X-API-Key', apiKey);

			const response = await fetch('https://api.api-ninjas.com/v1/babynames', {
				method: 'GET',
				headers: headers
			});
			const data = await response.json();
			console.log(data);
			babyNames = data;
			console.log(babyNames);
		} catch (error) {
			console.error('Error fetching baby names:', error.message);
		}
	}

	function setFilterSettings() {
		filterSettings = true;
		initialSettings = false
	}

	function isSelectionMade() {
		selectionMade = babyNameType !== '';
	}
</script>

<div class="card flex justify-center h-full items-center p-12">
	<div class="space-y-5">
		<h1 class="h1">BabyBabble</h1>
		<p>Swipe names for the big day!</p>

		{#if initialSettings}
			<h2>Let's start with your name (if you want)</h2>
			<div>
				<p>Your last name is: {lastName}</p>
				<input bind:value={lastName} />
			</div>
			<button class="btn variant-filled" on:click={setFilterSettings}>Continue</button>
		{/if}
		{#if filterSettings}
			<label for="settings">What kind of names are you looking for?</label>
			<select
				name="settings"
				id="settings"
				bind:value={babyNameType}
				on:change={isSelectionMade}
				required
			>
				<option value="boy">Boy Names</option>
				<option value="girl">Girl Names</option>
				<option value="neutral">Neutral Names</option>
			</select>

			<label for="popularNames" class="flex">
				<input class="checkbox mr-2" type="checkbox" bind:checked={popular} />
				<p>Search by Popular Names?</p>
			</label>

			<button class="btn variant-soft-secondary" disabled={!selectionMade} on:click={getBabyNames}
				>Let's get some names!</button
			>
		{/if}
	</div>
</div>
