<script>
  import { doFetch } from './Common.js'

  import { onMount } from 'svelte'

  import RideList from './RideList.svelte'
  import Chart from 'svelte-frappe-charts'

  export let onEdit
  
  let qresult = null
  let qresult2 = null
  let heatmapDataKms = null
  let heatmapDataAlt = null
  let heatmapColorsKms
  let heatmapColorsAlt

  onMount(async () => {
    await doList()
  })

  async function doList() {
    //id = '' // revert to Add mode

    qresult = await doFetch(
      'select id, ride_date, km, alt_gain, description, weather, route_id, bike_id from rides where ride_date >= curdate() - interval 1 month order by ride_date desc limit 7',
    )

    qresult2 = await doFetch(
      "select 'W', count(*), sum(km), sum(alt_gain) from rides where ride_date >= curdate() - interval 1 week" +
        ' union' +
        " select 'M', count(*), sum(km), sum(alt_gain) from rides where ride_date >= curdate() - interval 1 month" +
        ' union' +
        " select 'Y', count(*), sum(km), sum(alt_gain) from rides where ride_date >= curdate() - interval 1 year",
    )

    // heatmap
    let data = await doFetch(
      'select ride_date, sum(km) as sumkm, sum(alt_gain) as sumalt from rides where ride_date >= curdate() - interval 1 year group by 1 order by 1',
    )
    let dateKms = {}
    let dateAlt = {}
    data.forEach((item) => (dateKms[item.ride_date] = item.sumkm))
    data.forEach((item) => (dateAlt[item.ride_date] = item.sumalt))
    // console.log(dateKms)
    let today = new Date() 
    today.setDate(today.getDate() + 1)
    let t365 = new Date()
    t365.setDate(t365.getDate() - 366)

    heatmapColorsKms = ['#ebedf0', '#c6e48b', '#7bc96f', '#239a3b', '#196127']
    heatmapDataKms = {
      dataPoints: dateKms,
      start: t365,
      end: today,
    }
    heatmapColorsAlt = ['#ebedf0', '#c0ddf9', '#73b3f3', '#3886e1', '#17459e']
    heatmapDataAlt = {
      dataPoints: dateAlt,
      start: t365,
      end: today,
    }
  }

  function handleEdit(ride) {
    onEdit(ride)
  }

</script>

<!-- <button type="button" on:click={doList}>List</button> -->

<h4>Last week</h4>
{#if qresult}
  <RideList rides={qresult} onEdit={(ride) => handleEdit(ride)} />
{/if}

<h4>Km</h4>
{#if heatmapDataKms}
  <Chart data={heatmapDataKms} type="heatmap" colors={heatmapColorsKms} height=150px />
{/if}

<h4>Alt (m)</h4>
{#if heatmapDataAlt}
  <Chart data={heatmapDataAlt} type="heatmap" colors={heatmapColorsAlt} height=160px />
{/if}

<h4>Total # Km m</h4>
<pre>
  {#if qresult2}
    <ul>
      {#each qresult2 as row}
        <li>
          {#each Object.values(row) as field}{field + ' '}{/each}
        </li>
      {/each}
    </ul>
  {/if}
</pre>
