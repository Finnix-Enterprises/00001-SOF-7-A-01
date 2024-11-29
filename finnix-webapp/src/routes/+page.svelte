<script>
  import { onMount } from "svelte";

  let parameters = {};
  let newParam1 = "";
  let newParam2 = 0;
  let newParam3 = false;

  const BACKEND_URL = "https://31bc-2401-4900-1c53-3df6-1866-78a4-d9b9-8b7e.ngrok-free.app/parameters";

  async function fetchParameters() {
    try {
      const response = await fetch(BACKEND_URL);
      if (!response.ok) {
        throw new Error("Failed to fetch parameters");
      }
      parameters = await response.json();
    } catch (error) {
      console.error("Error fetching parameters:", error);
    }
  }

  async function updateParameters() {
    const updates = {
      param1: newParam1,
      param2: parseInt(newParam2),
      param3: newParam3 === "true" ? true : false,
    };

    try {
      const response = await fetch(BACKEND_URL, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(updates),
      });
      
      if (!response.ok) {
        throw new Error("Failed to update parameters");
      }
      
      await fetchParameters(); // Refresh the parameters after update
    } catch (error) {
      console.error("Error updating parameters:", error);
    }
  }

  onMount(fetchParameters);
</script>

<main class="min-h-screen bg-gray-50 flex flex-col justify-center items-center p-6 space-y-6">
  <div class="max-w-3xl w-full bg-white p-6 rounded-lg shadow-xl">
    <h1 class="text-4xl font-bold text-center text-red-600 mb-6">Finnix WebApp</h1>

    <div class="space-y-4">
      <h3 class="text-2xl text-gray-700">Current Parameters:</h3>
      <pre class="bg-gray-100 p-4 rounded-lg text-sm">{JSON.stringify(parameters, null, 2)}</pre>
    </div>

    <div class="mt-6 space-y-4">
      <h3 class="text-2xl text-gray-700">Update Parameters:</h3>
      <div class="space-y-4">
        <label class="block">
          <span class="text-gray-700">Relay Stats:</span>
          <input bind:value={newParam1} placeholder="New Value for Param1" class="mt-1 p-2 w-full border border-gray-300 rounded-lg shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500" />
        </label>
        <label class="block">
          <span class="text-gray-700">Delay:</span>
          <input bind:value={newParam2} type="number" placeholder="New Value for Param2" class="mt-1 p-2 w-full border border-gray-300 rounded-lg shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500" />
        </label>
        <label class="block">
          <span class="text-gray-700">Other:</span>
          <select bind:value={newParam3} class="mt-1 p-2 w-full border border-gray-300 rounded-lg shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500">
            <option value="true">True</option>
            <option value="false">False</option>
          </select>
        </label>
        <button on:click={updateParameters} class="w-full bg-blue-600 text-white py-2 rounded-lg shadow-md hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500 transition duration-300 ease-in-out">Update</button>
      </div>
    </div>
  </div>
</main>
