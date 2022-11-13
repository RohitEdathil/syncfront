<script lang="ts">
  import hcode from "highlighted-code";
  import { textStore } from "../store";

  export let listenMode: boolean;

  hcode.useTheme("github-dark");
</script>

<!-- svelte-ignore avoid-is -->
<textarea
  bind:value={$textStore}
  is="highlighted-code"
  spellcheck="false"
  disabled={listenMode ? true : false}
/>

<style>
  textarea {
    margin: 10px;
    padding: 10px;
    resize: none;
    flex: 4;
    border: 0;
    border-radius: 9px;
    background-color: #05060c !important;
    font-family: monospace;
    font-size: 17px;
  }
</style>
