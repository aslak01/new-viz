<script>
  import { LayerCake, ScaledSvg, Html, Svg } from 'layercake'
  import { feature } from 'topojson-client'
  import { geoAlbersUsa, geoNaturalEarth1 } from 'd3-geo'
  import { scaleQuantize } from 'd3-scale'
  import { format } from 'd3-format'

  import MapSvg from '$lib/chart-components/Map.svg.svelte'
  import Tooltip from '$lib/chart-components/Tooltip.html.svelte'

  import worldMap from '$lib/data/custom.geo-2.json'

  import usStates from '$lib/data/us-states.topojson.json'
  import stateData from '$lib/data/us-states-data.json'

  const colorKey = 'myValue'

  /* --------------------------------------------
   * Create lookups to more easily join our data
   * `dataJoinKey` is the name of the field in the data
   * `mapJoinKey` is the name of the field in the map file
   */
  const dataJoinKey = 'name'
  const mapJoinKey = 'name'
  const dataLookup = new Map()

  // const geojson = feature(usStates, usStates.objects.collection)
  const geojson = worldMap
  // console.log(worldMap)
  console.log(geojson)
  const projection = geoNaturalEarth1

  stateData.forEach((d) => {
    dataLookup.set(d[dataJoinKey], d)
  })

  let evt
  let hideTooltip = true

  // Create a flat array of objects that LayerCake can use to measure
  // extents for the color scale
  const flatData = geojson.features.map((d) => d.properties)
  // const colors = ['#ffdecc', '#ffc09c', '#ffa06b', '#ff7a33']
  const colors = [
    '#f7fcfd',
    '#e0ecf4',
    '#bfd3e6',
    '#9ebcda',
    '#8c96c6',
    '#8c6bb1',
    '#88419d',
    '#810f7c',
    '#4d004b'
  ]

  const addCommas = format(',')
</script>

<div class="chart-container">
  <!-- zRange={colors}
  z={(d) => dataLookup.get(d[mapJoinKey])[colorKey]}
  zScale={scaleQuantize()} -->
  <LayerCake
    data={worldMap}
    {flatData}
    zRange={colors}
    z={(d) => d.gdp_md_est / d.pop_est}
    zScale={scaleQuantize()}
  >
    <Svg>
      <MapSvg {projection} />
    </Svg>
    <!-- on:mousemove={(event) => (evt = hideTooltip = event)}
      on:mouseout={() => (hideTooltip = true)} -->

    <!-- <Html pointerEvents={false}>
      {#if hideTooltip !== true}
        <Tooltip {evt} let:detail> -->
    <!-- For the tooltip, do another data join because the hover event only has the data from the geography data -->
    <!-- {@const tooltipData = {
            ...detail.props,
            ...dataLookup.get(detail.props[mapJoinKey])
          }}
          {#each Object.entries(tooltipData) as [key, value]}
            {@const keyCapitalized = key.replace(/^\w/, (d) => d.toUpperCase())}
            <div class="row">
              <span>{keyCapitalized}:</span>
              {typeof value === 'number' ? addCommas(value) : value}
            </div>
          {/each}
        </Tooltip>
      {/if}
    </Html> -->
  </LayerCake>
</div>

<style>
  /*
    The wrapper div needs to have an explicit width and height in CSS.
    It can also be a flexbox child or CSS grid element.
    The point being it needs dimensions since the <LayerCake> element will
    expand to fill it.
  */
  .chart-container {
    width: 500px;
    height: 300px;
  }
</style>
