<script>
  import { doFetch } from './Common.js'

  import { onMount } from 'svelte'

  export let ride
  export let onDone

  let id = ''
  let ride_date = new Date()
  ride_date.setHours(ride_date.getHours() + 10)
  ride_date = ride_date.toISOString().split('T')[0]
  let route_id = '10'
  let bike_id = '10'
  let km = 21
  let alt_gain = 167
  let description = 'SoB'
  let weather = ''

  if(ride !== undefined) {
    console.log('editing')
    console.log(ride)
    id = ride.id
    ride_date = ride.ride_date
    route_id = ride.route_id
    bike_id = ride.bike_id
    km = ride.km
    alt_gain = ride.alt_gain
    description = ride.description
    weather = ride.weather
  }

  let qresult = null

  let routes = []
  let bikes = []

  onMount(async () => {
    routes = await doFetch('select id, name from routes order by name')
    bikes = await doFetch('select id, name from bikes order by name')
  })

  //fetch('https://www.artspace7.com.au/dsql/json_helper_get.php?db=art25285_rides2&sql=select%20*%20from%20bikes')

  async function doAddOrUpdate() {
    const sql =
      id === ''
        ? 'INSERT INTO rides (route_id, ride_date, km, alt_gain, description, weather, bike_id) ' +
          'values (' +
          route_id +
          ",'" +
          ride_date +
          "'," +
          km +
          ',' +
          alt_gain +
          ",'" +
          description +
          "','" +
          weather +
          "'," +
          bike_id +
          ')'
        : 'REPLACE INTO rides (id, route_id, ride_date, km, alt_gain, description, weather, bike_id) ' +
          'values (' +
          id +
          ',' +
          route_id +
          ",'" +
          ride_date +
          "'," +
          km +
          ',' +
          alt_gain +
          ",'" +
          description +
          "','" +
          weather +
          "'," +
          bike_id +
          ')'
    qresult = await doFetch(sql)
    console.log(qresult)
    onDone()
  }

</script>

<style>
  input {
    width: 90%;
  }
</style>

<label>Date</label>
<input type="date" bind:value={ride_date} />

<label>Route</label>
<select id="id_route" bind:value={route_id}>
  {#each routes as route}
    <option value={route.id}>{route.name}</option>
  {/each}
</select>

<label>Bike</label>
<select id="id_bike" bind:value={bike_id}>
  {#each bikes as bike}
    <option value={bike.id}>{bike.name}</option>
  {/each}
</select>

<label>km</label>
<input type="number" bind:value={km} />

<label>alt</label>
<input type="number" bind:value={alt_gain} />

<label>Description</label>
<input bind:value={description} />

<label>Weather</label>
<input bind:value={weather} />

<button type="button" on:click={doAddOrUpdate}>
  {#if id === ''}Add Ride{:else}Update{/if}
</button>
