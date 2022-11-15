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

  let isGreen = true;

  let flagCount = 0;
  let greenFlagCount = 0;

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
    const wsMessage = JSON.parse(ev.data);

    switch (wsMessage.type) {
      case "code-state":
        textStore.set(wsMessage.data);
        break;

      case "flag-count":
        flagCount = wsMessage.data;
        break;

      case "green-flag-count":
        greenFlagCount = wsMessage.data;
        break;

      default:
        message.set(wsMessage.data);
        break;
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

  function switchHandler() {
    isGreen = !isGreen;
    ws.send(JSON.stringify({ type: "flag-switch" }));
  }
</script>

<div id="container">
  <TopBar {id} {connected} />
  <div id="inner" on:keypress={handleShortcuts}>
    <TextBox {listenMode} />
    <StatBar
      {copyHandler}
      {sendHandler}
      {switchHandler}
      {connected}
      {listenMode}
      {isGreen}
      {flagCount}
      {greenFlagCount}
    />
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
