<script lang="ts">
  import { WSServerUrl } from "../../constants";
  import { message } from "../popup/store";
  import StatBar from "./components/StatBar.svelte";
  import TextBox from "./components/TextBox.svelte";
  import TopBar from "./components/TopBar.svelte";
  import { textStore } from "./store";

  export let id: string;
  let text: string = "";

  let listenMode = true;
  let connected = false;

  // Checks for stored credentials
  const storedId = window.localStorage.getItem("id");
  const storedSecret = window.localStorage.getItem("secret");

  // Switches to broadcaster mode if credentials are found
  if (id == storedId) {
    listenMode = false;
  }

  let ws: WebSocket;

  connectToWs();
  function connectToWs() {
    console.log("Connecting to websocket");

    if (listenMode == true) {
      ws = new WebSocket(`${WSServerUrl}/listen/${id}`);
    } else {
      ws = new WebSocket(`${WSServerUrl}/attach/${id}?secret=${storedSecret}`);
    }
  }

  function retry() {
    // Retry connection after 5 seconds
    setTimeout(() => {
      connectToWs();
    }, 5000);
  }

  ws.onopen = (ev) => {
    connected = true;
    console.log("Connected to websocket");
  };

  ws.onclose = (ev) => {
    connected = false;
    console.log("Disconnected from websocket");

    retry();
  };

  ws.onmessage = (ev) => {
    const message = JSON.parse(ev.data);

    if (message.type == "code-state") {
      textStore.set(message.data);
    } else if (message.type == "error") {
      message.set(message.data);
    }
  };

  textStore.subscribe((value) => {
    text = value;
  });

  function handleShortcuts(e: KeyboardEvent) {
    if (e.key == "Enter" && e.shiftKey) {
      e.preventDefault();
      sendHandler();
    }
  }

  function copyHandler() {
    navigator.clipboard.writeText(text);
    message.set("Text Copied to clipboard");
  }
  function sendHandler() {
    if (text == undefined) return;
    ws.send(JSON.stringify({ type: "code-state", data: text }));
  }
</script>

<div id="container">
  <TopBar {id} {connected} />
  <div id="inner" on:keypress={handleShortcuts}>
    <TextBox {listenMode} />
    <StatBar {copyHandler} {sendHandler} {connected} {listenMode} />
  </div>
</div>

<style>
  @media (max-width: 768px) {
    #inner {
      flex-direction: column !important;
    }
  }

  #container {
    display: flex;
    flex-direction: column;
    justify-content: start;
    margin: 5px;
    height: 100%;
  }
  #inner {
    display: flex;
    flex-direction: row;
    height: 100%;
  }
</style>
