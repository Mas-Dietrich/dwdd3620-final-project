<script>
	import { Accordion, AccordionItem } from '@skeletonlabs/skeleton'
	import {icons} from '$lib/icons/icons.js'
	import {ProgressBar} from '@skeletonlabs/skeleton';

	let initialSettings = true

	let lastName = '';

	let filterSettings = false;

	let babyNameType = '';

	let popular = false;

	let selectionMade = false;

	let babyNames = [];

	let likedNames = [];

	let currentBabyNameIndex = 0

	let apiKey = '2LP0z26UuHCOic1ZWZ/x5w==7WiMg5Quumm85tl6';

	let showCards = false

	let showMoreNamesPrompt = false

	let isLoading = false

	async function getBabyNames() {
		try {
			isLoading = true // Set loading screen while API calls are made
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
		await getNameDefinition()
		displayBabyNames()

	} catch(error) {
		console.error('Error fetching baby names:', error.message)
	} finally {
		isLoading = false
	}
}

async function getNameDefinition() {
	try {
		for (let i=0; i < babyNames.length; i++) {
			const name = babyNames[i];
			const response = await fetch(`https://api.api-ninjas.com/v1/dictionary?word=${name}`,{
				method: 'GET',
				headers: {
					'X-API-Key': apiKey
				}
			})
			const data = await response.json();
			if(data.definition) {
				babyNames[i] = {
					name: name,
					definition: data.definition
				}
			} else {
				babyNames[i] = {
					name: name,
					definition: "No name definition available"
				}
			}
			const celebrityResponse = await fetch(`https://api.api-ninjas.com/v1/celebrity?name=${name}`, {
				method: 'GET',
				headers: {
					'X-API-Key': apiKey
				}
			})
			const celebrityData = await celebrityResponse.json()
			const celebrityNames = celebrityData.map(celebrity => celebrity.name)
			babyNames[i].celebrities = celebrityNames.slice(0,5)
		}
	} catch (error) {
		console.error('Error fetching definitions:', error.message)
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

	function handleLikedBabyNames() {
		likedNames.push(babyNames[currentBabyNameIndex]);
		console.log(likedNames)
		nextName()
	}

	function nextName() {
		if(currentBabyNameIndex < babyNames.length - 1) {
			currentBabyNameIndex++
		} else {
			currentBabyNameIndex = 0
			if (babyNames.length >= 10) {
				showMoreNamesPrompt = true
				showCards = false
			}
		}
	}

	function handleDislikedBabyNames() {
		nextName()
	}

	async function loadMoreBabyNames() {
		showMoreNamesPrompt = false
		await getBabyNames()
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

	{#if isLoading}
		<div class="w-3/4 mx-aut0 my-12 p-8 variant-ghost-primary rounded-full animate-bounce">
			<h2 class="text-center text-3xl mb-8">Babies Names Being Babbled...</h2>
			<div class="w-1/2 m-auto">
				<ProgressBar value={undefined}/>
			</div>
		</div>
	{/if}

	<div class="flex justify-center flex-col w-3/4">
		{#if !initialSettings && !filterSettings && !showMoreNamesPrompt}
				<div class="card p-8 my-8 relative">
					<h2 class="text-3xl font-bold text-center">{babyNames[currentBabyNameIndex].name} {lastName}</h2>
					<div class="w-3/4 mx-auto my-4">
						<Accordion>
							<AccordionItem>
								<svelte:fragment slot="lead">{@html icons.book}</svelte:fragment>
								<svelte:fragment slot="summary">Definition</svelte:fragment>
								<svelte:fragment slot="content">
									<p class="my-4 italic w-3/4">{babyNames[currentBabyNameIndex].definition}</p>
								</svelte:fragment>
							</AccordionItem>
							<AccordionItem>
								<svelte:fragment slot="lead">{@html icons.camera}</svelte:fragment>
								<svelte:fragment slot="summary">Celebrities With This Name</svelte:fragment>
								<svelte:fragment slot="content">
									{#if babyNames[currentBabyNameIndex].celebrities && babyNames[currentBabyNameIndex].celebrities.length > 0}
										<h4 class="underline">Celebrities with this name:</h4>
											<ol class="list flex flex-col">
											{#each babyNames[currentBabyNameIndex].celebrities as celebrity, index}
												<li>
													<span>{index + 1}.</span>
													<span class="flex-auto">{celebrity}</span>
												</li>
											{/each}
											</ol>
									{:else}
											<p>No celebrity names available</p>
									{/if}
								</svelte:fragment>
							</AccordionItem>
						</Accordion>
					</div>
					<div class="absolute left-[5%] bottom-[50%] text-red-600 text-xl cursor-pointer">
						<button class="btn" on:click={handleDislikedBabyNames}>X</button>
					</div>

					<div class="absolute bottom-[50%] right-[5%] text-green-600 text-xl cursor-pointer">
						<button class="btn"  on:click={handleLikedBabyNames}>✅</button>
					</div>
				</div>
		{/if}
		{#if showMoreNamesPrompt}
		<div class="text-center my-4">
			<button class="btn variant-filled hover:animate-pulse" on:click={loadMoreBabyNames}>See More Baby Names</button>
		</div>
	{/if}
	</div>
</div>
