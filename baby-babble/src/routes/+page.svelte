<script>
	let lastName = ''

	let filterSettings = false

	let babyNameType = ''

	let popular = false

	let selectionMade = false


	function setFilterSettings() {
		filterSettings = true
	}

	function showSettings() {
		console.log(
			lastName, babyNameType, popular
		)
	}

	function isSelectionMade() {
		selectionMade = babyNameType !== ''
	}


</script>

<div class="flex justify-center h-full items-center p-12">
	<div class="space-y-5">
		<h1 class="h1">BabyBabble</h1>
		<p>Swipe names for the big day!</p>

		<h2>Let's start with your name (if you want)</h2>
		<div>
			<p>Your last name is: {lastName}</p>
			<input bind:value={lastName}/>
		</div>
		<button class="btn variant-filled" on:click={setFilterSettings}>Continue</button>

		{#if filterSettings}
			<label for="settings">What kind of names are you looking for?</label>
			<select name="settings" id="settings" bind:value={babyNameType} on:change={isSelectionMade} required>
				<option value="boy">Boy Names</option>
				<option value="girl">Girl Names</option>
				<option value="neutral">Neutral Names</option>
			</select>

			<label for="popularNames" class="flex">
				<input class="checkbox mr-2" type="checkbox" bind:checked={popular} />
				<p>Search by Popular Names?</p>
			</label>

			<button class="btn variant-soft-secondary" disabled={!selectionMade} on:click={showSettings}>Let's get some names!</button>

		{/if}
	</div>
</div>
