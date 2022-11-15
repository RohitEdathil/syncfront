<script>
  export let copyHandler;
  export let sendHandler;
  export let switchHandler;

  export let connected;
  export let listenMode;
  export let isGreen;

  export let flagCount;
  export let greenFlagCount;

  let width = 0;

  $: {
    if (flagCount == 0) {
      width = 0;
    } else {
      width = (greenFlagCount / flagCount) * 100;
    }
  }
</script>

<div id="container">
  {#if !listenMode}
    <p>Watching</p>
    <h1>{flagCount}</h1>

    <div id="stat-bar">
      {greenFlagCount}
      <div id="red-bg">
        <div id="green-fg" class="smooth" style="width: {width}%;" />
      </div>
      {flagCount - greenFlagCount}
    </div>

    <button
      title="Shortcut: Ctrl + Enter"
      on:click={sendHandler}
      disabled={!connected}>Send</button
    >
  {:else}
    <p>Are you following?</p>
    <button
      id="switcher"
      on:click={switchHandler}
      style={isGreen ? "" : "color:rgb(212, 0, 0)"}
    >
      {isGreen ? "Yes" : "No"}</button
    >
  {/if}

  <button on:click={copyHandler}>Copy text</button>
</div>

<style>
  p {
    text-align: center;
    margin-bottom: 0px;
  }
  h1 {
    text-align: center;
    margin-top: 0px;
    margin-bottom: 8px;
  }
  button {
    margin: 10px 20px;
    background-color: rgb(0, 94, 255);
  }

  #container {
    background-color: #05060c;
    flex: 1;
    margin: 10px;
    border-radius: 10px;
    display: flex;
    flex-direction: column;
  }
  #stat-bar {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    margin: 10px;
  }
  #red-bg {
    background-color: rgb(212, 0, 0);
    width: 50%;
    height: 10px;
    margin: 0px 10px;
    max-width: 200px;
    border-radius: 10px;
  }
  #green-fg {
    background-color: rgb(48, 189, 48);
    width: 50%;
    height: 10px;
    border-radius: 10px;
  }

  #switcher {
    color: rgb(48, 189, 48);
    margin-bottom: 15px;
    user-select: none;
    font-size: 50px;
    background-color: transparent;
  }

  #switcher:focus,
  #switcher:focus-visible {
    outline: none;
  }
</style>
