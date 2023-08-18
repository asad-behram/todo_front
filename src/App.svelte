<script>
  import Home from "./components/Home.svelte";
  import NavBar from "./components/NavBar.svelte";
  import Show from "./components/Show.svelte";
  import Router from "svelte-spa-router";

  const routes = {
    "/": Home,
    "/task/:id": Show,
  };
</script>

<NavBar />

<main>
  <Router {routes} />
</main>
