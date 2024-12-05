<script>
	import { onMount } from 'svelte';

	let parameters = {}; // Variable to store fetched parameters
	let newParam1 = '';
	let newParam2 = 0;
	let newParam3 = 'false';

	const BACKEND_URL = ' https://97ab3baacbd93651bf2cbbf07538ca09.serveo.net/parameters';//ADD SERVEO.NET LINK HERE

	// Function to fetch parameters from the backend
	async function fetchParameters() {
		try {
			const response = await fetch(BACKEND_URL);
			if (!response.ok) {
				throw new Error('Failed to fetch parameters');
			}
			const data = await response.json();
			console.log('Fetched parameters:', data);
			parameters = data; // Update local state
		} catch (error) {
			console.error('Error fetching parameters:', error);
		}
	}

	// Function to update parameters
	async function updateParameters() {
		const updates = {
			param1: newParam1,
			param2: parseInt(newParam2),
			param3: newParam3 === 'true'
		};

		try {
			const response = await fetch(BACKEND_URL, {
				method: 'POST',
				headers: {
					'Content-Type': 'application/json'
				},
				body: JSON.stringify(updates)
			});

			if (!response.ok) {
				throw new Error('Failed to update parameters');
			}

			// Optionally, you could display a success message or handle success state here.
		} catch (error) {
			console.error('Error updating parameters:', error);
		}
	}

	// Fetch parameters on initial page load
	onMount(fetchParameters);
</script>

<main class="flex min-h-screen flex-col items-center justify-center space-y-6 bg-gray-50 p-6">
	<div class="w-full max-w-3xl rounded-lg bg-white p-6 shadow-xl">
		<h1 class="mb-6 text-center text-4xl font-bold text-red-600">Finnix WebApp</h1>

		<!-- Display fetched parameters -->
		<div class="space-y-4">
			<h3 class="text-2xl text-gray-700">Current Parameters:</h3>
			<pre class="rounded-lg bg-gray-100 p-4 text-sm">{JSON.stringify(parameters, null, 2)}</pre>
		</div>

		<!-- Button to fetch parameters -->
		<div class="mt-6 flex justify-center">
			<button
				on:click={fetchParameters}
				class="rounded-lg bg-gradient-to-r from-green-400 to-blue-500 px-4 py-2 font-bold text-white shadow-lg transition duration-300 ease-in-out hover:from-green-500 hover:to-blue-600 focus:outline-none focus:ring-4 focus:ring-blue-300"
			>
				Refresh Parameters
			</button>
		</div>

		<!-- Form to update parameters -->
		<div class="mt-6 space-y-4">
			<h3 class="text-2xl text-gray-700">Update Parameters:</h3>
			<div class="space-y-4">
				<label class="block">
					<span class="text-gray-700">Relay Stats:</span>
					<input
						bind:value={newParam1}
						placeholder="New Value for Param1"
						class="mt-1 w-full rounded-lg border border-gray-300 p-2 shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500"
					/>
				</label>
				<label class="block">
					<span class="text-gray-700">Delay:</span>
					<input
						bind:value={newParam2}
						type="number"
						placeholder="New Value for Param2"
						class="mt-1 w-full rounded-lg border border-gray-300 p-2 shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500"
					/>
				</label>
				<label class="block">
					<span class="text-gray-700">Other:</span>
					<select
						bind:value={newParam3}
						class="mt-1 w-full rounded-lg border border-gray-300 p-2 shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500"
					>
						<option value="true">True</option>
						<option value="false">False</option>
					</select>
				</label>
				<button
					on:click={updateParameters}
					class="w-full rounded-lg bg-blue-600 py-2 text-white shadow-md transition duration-300 ease-in-out hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500"
				>
					Update
				</button>
			</div>
		</div>
	</div>
</main>
