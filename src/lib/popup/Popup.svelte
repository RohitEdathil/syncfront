<script lang="ts">
  import { onDestroy } from "svelte";
  import { message } from "./store";

  let showMessage: string;
  let hidden = true;

  const unsubscribe = message.subscribe((value) => {
    if (value == "") return;

    showMessage = value;
    hidden = false;

    console.log("showMessage", showMessage);

    setTimeout(() => {
      hidden = true;
      message.set("");
    }, 2000);
  });

  onDestroy(unsubscribe);
</script>

<div class={hidden ? "hidden smooth" : "smooth"}>
  <p>{showMessage}</p>
</div>

<style>
  .hidden {
    bottom: -100px;
  }

  div {
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    align-items: center;
    width: 100%;
    position: fixed;

    bottom: 10px;
  }

  p {
    background-color: #3d3c3c;
    width: min-content;
    min-width: 200px;
    text-align: center;
    padding: 12px;
    border-radius: 10px;
  }
</style>
