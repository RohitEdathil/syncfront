<script lang="ts">
  function join() {
    console.log("join");
  }

  function create() {
    console.log("create");
  }
</script>

<div id="container">
  <h1>TypeSync</h1>
  <h2>An easy way to share code live and get live feedback</h2>
  <button id="create" on:click={create}>Create</button>

  <h3>Or</h3>

  <div id="btns">
    <input type="text" placeholder="Enter code" />
    <button id="join" on:click={join}>Join</button>
  </div>
</div>

<style>
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
</style>
