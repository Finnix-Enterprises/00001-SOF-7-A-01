<script>
    import { onMount } from 'svelte';
  
    let parameters = {};  // Store parameters fetched from the backend
    let newParam1 = "";
    let newParam2 = 0;
    let newParam3 = false;
  
    // Fetch parameters from the backend
    async function fetchParameters() {
      const response = await fetch("http://localhost:3000/parameters");  // Backend URL (on port 3000)
      parameters = await response.json();
    }
  
    // Update parameters on the backend
    async function updateParameters() {
      const updates = {
        param1: newParam1,
        param2: parseInt(newParam2),
        param3: newParam3 === "true" ? true : false,
      };
      await fetch("http://localhost:3000/parameters", {  // Backend URL (on port 3000)
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(updates),  // Send updated parameters to the backend
      });
      await fetchParameters();  // Refresh the parameters after update
    }
  
    onMount(fetchParameters);  // Call fetchParameters when the page loads
  </script>
  
  <h1>Parameter Management</h1>
  
  <div>
    <h3>Current Parameters:</h3>
    <pre>{JSON.stringify(parameters, null, 2)}</pre>
  </div>
  
  <div>
    <h3>Update Parameters:</h3>
    <label>
      Relay_stats: <input bind:value={newParam1} placeholder="New Value for Param1" />
    </label>
    <label>
      Delay: <input bind:value={newParam2} type="number" placeholder="New Value for Param2" />
    </label>
    <label>
      other:
      <select bind:value={newParam3}>
        <option value="true">True</option>
        <option value="false">False</option>
      </select>
    </label>
    <button on:click={updateParameters}>Update</button>
  </div>
  