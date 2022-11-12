<script lang="ts">
  import Home from "./lib/home/Home.svelte";
  import TypeSync from "./lib/typesync/TypeSync.svelte";

  import { Router, Route } from "svelte-navigator";
</script>

<main>
  <Router primary={false}>
    <Route path="/" component={Home} />

    <Route path="/:id" let:params>
      <TypeSync id={params.id} />
    </Route>
  </Router>
</main>

<style>
  main {
    height: 100%;
    width: 100%;
    display: flex;
  }
</style>
