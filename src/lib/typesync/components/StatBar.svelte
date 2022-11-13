<script>
  export let copyHandler;
  export let sendHandler;
  export let connected;
  export let listenMode;
</script>

<div>
  <button on:click={copyHandler}>Copy text</button>

  {#if listenMode == false}
    <button on:click={sendHandler} disabled={!connected}>Send</button>
  {/if}
</div>

<style>
  div {
    background-color: #05060c;
    flex: 1;
    margin: 10px;
    border-radius: 10px;
    display: flex;
    flex-direction: column;
  }
  button {
    margin: 10px 20px;
    background-color: rgb(0, 94, 255);
  }
</style>
