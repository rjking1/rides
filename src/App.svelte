<script>
  import NavButton from './NavButton.svelte'
  import AddPage from './AddPage.svelte'
  import ListPage from './ListPage.svelte'
  import QueryPage from './QueryPage.svelte'

  const pageMap = {
    add: AddPage,
    list: ListPage,
    query: QueryPage,
  }

  let pageName = 'list'

  let ride = undefined

  function handleEdit(editRide) {
    ride = editRide
    pageName = 'add'
  }

  function handleDone() {
    ride = undefined
    pageName = 'list'
  }

</script>

<!-- =============== HTML =============== -->

<h1>Ride tracker</h1>

<nav>
  <NavButton bind:pageName name="list">List</NavButton>
  <NavButton bind:pageName name="add">Add</NavButton>
  <NavButton bind:pageName name="query">Query</NavButton>
</nav>

<main>
  {#if pageName == 'add'}  
    <svelte:component this={ pageMap[pageName] } ride={ride} onDone={handleDone} />
  {:else if pageName == 'list'}  
    <svelte:component this={ pageMap[pageName] } onEdit={handleEdit} />
  {:else}
    <svelte:component this={ pageMap[pageName] } />
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

