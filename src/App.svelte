<script lang="ts">
  import Home from "./lib/pages/Home.svelte";
  import TypeSync from "./lib/pages/TypeSync.svelte";

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
</style>
