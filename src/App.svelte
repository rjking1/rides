<script>
  import { dbName, page, permissions } from "./Stores.js";

  import NavButton from "./NavButton.svelte";
  import AddPage from "./AddPage.svelte";
  import ListPage from "./ListPage.svelte";
  import QueryPage from "./QueryPage.svelte";
  import LoginPage from "./LoginPage.svelte";

  const pageMap = {
    login: LoginPage,
    add: AddPage,
    list: ListPage,
    query: QueryPage,
  };

  const urlParams = new URLSearchParams(window.location.search);
  if (urlParams.has("db")) {
    $dbName = urlParams.get("db");
  }

  let ride = undefined;

  function handleEdit(editRide) {
    ride = editRide;
    $page = "add";
  }

  function handleDone() {
    ride = undefined;
    $page = "list";
  }
</script>

<!-- =============== HTML =============== -->

<h1>
  <span style="text-align:left;"> Ride tracker </span>
  <span style="float:right; font-size: medium;">
    {#if $permissions.u_name}
      DB: {$dbName}<br />{$permissions.u_name}
    {/if}
  </span>
</h1>
<nav>
  <NavButton name="login">Login</NavButton>
  <NavButton name="list">List</NavButton>
  <NavButton name="add">Add</NavButton>
  <NavButton name="query">Query</NavButton>
</nav>

<main>
  {#if $page == "add"}
    <svelte:component this={pageMap[$page]} {ride} onDone={handleDone} />
  {:else if $page == "list"}
    <svelte:component this={pageMap[$page]} onEdit={handleEdit} />
  {:else}
    <svelte:component this={pageMap[$page]} />
  {/if}
</main>

<style>
  main {
    padding: 10px;
  }
  nav {
    display: flex;
    align-items: center;
    background-color: cornflowerblue;
    padding: 10px;
  }
</style>
