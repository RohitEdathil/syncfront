<script lang="ts">
  import Home from "./lib/home/Home.svelte";
  import TypeSync from "./lib/typesync/TypeSync.svelte";

  import { Router, Route } from "svelte-navigator";
  import Popup from "./lib/popup/Popup.svelte";
</script>

<main>
  <Router primary={false}>
    <Route path="/" component={Home} />

    <Route path="/:id" let:params>
      <TypeSync id={params.id} />
    </Route>
  </Router>

  <Popup />
</main>

<style>
  main {
    height: 100%;
    width: 100%;
    display: flex;
    flex-direction: column;
  }
</style>
