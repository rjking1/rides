<script>
  import RideHeader from './RideHeader.svelte'
  import Ride from './Ride.svelte'

  export let rides
  export let onEdit

  function handleEdit(ride) {
    onEdit(ride)
  }
</script>

<table>
  <RideHeader />
  {#each rides as ride}
    <Ride bind:ride onEdit={() => handleEdit(ride)} />
  {/each}
</table>
