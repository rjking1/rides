<script>
  export let ride
  export let onEdit

  function handleEdit() {
    onEdit(ride)
  }
</script>

<tr>
  <td>{ride.ride_date}</td>
  <td>{ride.km}</td>
  <td>{ride.alt_gain}</td>
  <td>{ride.description}</td>
  <td>{ride.weather}</td>

  <td>
    <button on:click={handleEdit}>✎ Edit</button>
  </td>
</tr>
