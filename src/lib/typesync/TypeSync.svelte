<script lang="ts">
  import TextBox from "./components/TextBox.svelte";
  export let id: string;
</script>

<div id="container">
  <div>
    <TextBox {id} />
  </div>
</div>

<style>
  #container {
    display: flex;
    flex-direction: column;
    justify-content: start;
  }
</style>
