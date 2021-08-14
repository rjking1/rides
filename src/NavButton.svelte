<script>
  import { loggedIn, page } from "./Stores.js";

  export let name;
  // export let pageName;

  function doLoginCheck() {
    if ($loggedIn == "false") {
      $page = "login";
      // } else if (name == "back") {
      //   if (canGoBack()) {
      //     $page = goBack();
      //   }
      // } else if (name == "index") {
      //   clearPageHistory();
      //   $page = gotoPage("index", "index");
    } else {
      $page = name;
    }
  }
</script>

<button class:active={$page === name} on:click={doLoginCheck}>
  <slot />
</button>

<style>
  button {
    --space: 0.5rem;
    background-color: white;
    border-radius: var(--space);
    height: 38px;
    margin-right: var(--space);
    padding: var(--space);
  }
  .active {
    background-color: yellow;
  }
</style>
