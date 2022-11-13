<script lang="ts">
  import { message } from "../../popup/store";

  export let id: string;
  export let connected: boolean;

  function copyCode() {
    navigator.clipboard.writeText(location.href);
    message.set("Link Copied to clipboard");
  }
</script>

<div>
  <h1>TypeSync</h1>
  <button on:click={copyCode} title="Click to copy link">{id}</button>
  <div
    class="dot {connected ? 'green' : 'red'}"
    title={connected ? "Connected" : "Disconnected"}
  />
</div>

<style>
  .dot {
    height: 10px;
    width: 10px;
    border-radius: 50%;
  }
  .green {
    background-color: #368b36;
  }
  .red {
    background-color: #c82222;
  }

  div {
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    align-items: center;
    width: 300px;
  }

  h1 {
    margin: 20px;
    font-size: 25px;
    color: orange;
  }
</style>
