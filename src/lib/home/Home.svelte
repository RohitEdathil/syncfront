<script lang="ts">
  import { BackendUrl } from "../../constants";
  import { useNavigate } from "svelte-navigator";
  import loadingGif from "../../assets/loading-gif.gif";

  let id: string;

  let loading = false;
  let error = false;

  const navigate = useNavigate();

  async function join() {
    // Ignore blank id
    if (id == undefined || id == "") return;

    // Check if id exists
    loading = true;
    const response = await fetch(`${BackendUrl}/api/code/${id}`);
    loading = false;

    // If id exists, navigate to it
    if (response.status == 200) {
      navigate(`/${id}`);
    } else {
      error = true;
    }
  }

  async function create() {
    // Create new id
    const response = await (await fetch(`${BackendUrl}/api/code/new`)).json();

    // Set id and secret as cookies
    window.localStorage.setItem("id", response.id);
    window.localStorage.setItem("secret", response.secret);

    // Navigate to created id
    navigate(`/${response.id}`);
  }

  function clearError() {
    console.log("Trying to clear error");
    error = false;
  }
</script>

<div id="container">
  <h1>TypeSync</h1>
  <h2>An easy way to share code live and get live feedback</h2>
  <button id="create" on:click={create}>Create</button>

  <h3>Or</h3>

  <div id="btns">
    <input
      type="text"
      placeholder="Enter code"
      on:input={clearError}
      bind:value={id}
      class={error ? "error smooth" : "smooth"}
    />

    <button id="join" on:click={join} class="smooth">
      {#if loading}
        <img src={loadingGif} style="height: 17px;" alt="" />
      {:else}
        Join
      {/if}
    </button>
  </div>
</div>

<style>
  .error {
    border-bottom: 3px solid red;
  }

  #btns {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
  }

  h1 {
    font-size: 100px;
    font-weight: 700;
    margin: 0;
    color: orange;
  }
  button {
    margin: 5px;
    width: 200px;

    font-weight: bold;
    background-color: rgb(0, 103, 206);
  }
  #create {
    background-color: rgb(0, 148, 0);
  }

  input {
    margin: 10px;
    width: 200px;
    border: none;
    border-radius: 8px;
    padding: 0.7em 1.2em;
    font-size: 1em;
  }

  #container {
    margin: auto;
    display: flex;
    flex-direction: column;
    padding: 0px 20px;
    align-items: center;
  }
</style>
