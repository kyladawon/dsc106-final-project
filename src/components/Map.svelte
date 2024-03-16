<script>
  import mapboxgl from 'mapbox-gl';
  import { onMount } from 'svelte';
  export let index;
  export let geoJsonToFit;
  import * as d3 from 'd3';

  mapboxgl.accessToken =
    'pk.eyJ1Ijoia3lsYXBwYXJrIiwiYSI6ImNsc21mNWwxNzBsc3oycXJ5NTAyMzZtamQifQ.FH00Q1Y2ARsOleB7tPzh0A';

  let container;
  let map;
  let citiesYears = {
    Montreal: { coordinates: [-73.5673, 45.5017], year: 1976 },
    Moscow: { coordinates: [37.6175, 55.7558], year: 1880 },
    'Los Angeles': { coordinates: [-118.2437, 34.0522], year: 1984 },
    Seoul: { coordinates: [126.978, 37.5665], year: 1988 },
    Barcelona: { coordinates: [2.1734, 41.3851], year: 1992 },
    Atlanta: { coordinates: [-84.388, 33.749], year: 1996 },
    Sydney: { coordinates: [151.2093, -33.8688], year: 2000 },
    Athens: { coordinates: [23.7275, 37.9838], year: 2004 },
    Beijing: { coordinates: [116.4074, 39.9042], year: 2008 },
  };
  let cityMarkers = [];
  
  let countries = {
    'United States': {medals: 1992},
    'China':  {medals: 679 }
  }
 



  let data = [];
  async function fetchData() {
        const resForward = await fetch('olympic.csv');
        const csvForward = await resForward.text();
        data = d3.csvParse(csvForward, d3.autoType);
    }

    const top10_no_host = [
        { 'code': 'URS', 'cnt': 1 },
        { 'code': 'GDR', 'cnt': 1 },
        { 'code': 'ROU', 'cnt': 1 },
        { 'code': 'GER', 'cnt': 1 },
        { 'code': 'GBR', 'cnt': 1 },
        { 'code': 'ITA', 'cnt': 1 } 
    ]

    const top10_host = [
        { 'code': 'AUS', 'cnt': 1 },
        { 'code': 'CHN', 'cnt': 1 },
        { 'code': 'RUS', 'cnt': 1 },
        { 'code': 'USA', 'cnt': 1 }
    ]

 

    const host_bad = [
        { 'code': 'ESP', 'cnt': 1 },
        { 'code': 'CAN', 'cnt': 1 },
        { 'code': 'KOR', 'cnt': 1 },
        { 'code': 'GRC', 'cnt': 1 }
    ]
    

  onMount(() => {
    fetchData();
    
    map = new mapboxgl.Map({
      container,
      style: 'mapbox://styles/mapbox/light-v11',
      center: [10, 30],
      zoom: 1.8,
      pitch: 0,
      bearing: 0,
      projection: 'mercator',
    });

    // map.on('load', () => {
    //   addMarkers();
    //   updateBounds();
    //   map.on('zoom', updateBounds);
    //   map.on('drag', updateBounds);
    //   map.on('move', updateBounds);



      
    // }); 

    map.on('load', () => {

      addMarkers();
      updateBounds();
      map.on('zoom', updateBounds);
      map.on('drag', updateBounds);
      map.on('move', updateBounds);

        // Add source for country polygons using the Mapbox Countries tileset
        // The polygons contain an ISO 3166 alpha-3 code which can be used to for joining the data
        // https://docs.mapbox.com/vector-tiles/reference/mapbox-countries-v1
        map.addSource('countries', {
            type: 'vector',
            url: 'mapbox://mapbox.country-boundaries-v1'
        });

        // Build a GL match expression that defines the color for every vector tile feature
        // Use the ISO 3166-1 alpha 3 code as the lookup key for the country shape
        const matchExpression = ['match', ['get', 'iso_3166_1_alpha_3']];

        // Calculate color values for each country based on 'hdi' value
        for (const row of top10_host) {
            // Convert the range of data values to a suitable color
            // const green = row['cnt'] * 255;
            // const color = `rgb(0, ${green}, 0)`;

            // matchExpression.push(row['code'], color);
            const gold = `#B7C9F2`; // Gold color
            matchExpression.push(row['code'], gold);
        }

        for (const row of top10_no_host) {
            // Convert the range of data values to a suitable color
            // const green = row['cnt'] * 255;
            // const color = `rgb(0, ${green}, 0)`;

            // matchExpression.push(row['code'], color);
            const blue = `#FBA834`; // Gold color
            matchExpression.push(row['code'], blue);
        }

        for (const row of host_bad) {
            // Convert the range of data values to a suitable color
            // const green = row['cnt'] * 255;
            // const color = `rgb(0, ${green}, 0)`;

            // matchExpression.push(row['code'], color);
            const green = `#81689D`; // Gold color
            matchExpression.push(row['code'], green);
        }


        // Last value is the default, used where there is no data
        matchExpression.push('rgba(0, 0, 0, 0)');

        // The mapbox.country-boundaries-v1 tileset includes multiple polygons for some
        // countries with disputed borders.  The following expression filters the
        // map view to show the "US" perspective of borders for disputed countries.
        // Other world views are available, for more details, see the documentation
        // on the "worldview" feature property at
        // https://docs.mapbox.com/data/tilesets/reference/mapbox-countries-v1/#--polygon---worldview-text
        const WORLDVIEW = 'US';
        const worldview_filter = [
            'all',
            ['==', ['get', 'disputed'], 'false'],
            [
                'any',
                ['==', 'all', ['get', 'worldview']],
                ['in', WORLDVIEW, ['get', 'worldview']]
            ]
        ];

        // Add layer from the vector tile source to create the choropleth
        // Insert it below the 'admin-1-boundary-bg' layer in the style
        map.addLayer(
            {
                'id': 'countries-join',
                'type': 'fill',
                'source': 'countries',
                'source-layer': 'country_boundaries',
                'paint': {
                    'fill-color': matchExpression
                },
                'filter': worldview_filter
            },
            'admin-1-boundary-bg'
        );
    });
  
  
  });

  function updateBounds() {
    const bounds = map.getBounds();
    geoJsonToFit.features[0].geometry.coordinates = [
      bounds._ne.lng,
      bounds._ne.lat,
    ];
    geoJsonToFit.features[1].geometry.coordinates = [
      bounds._sw.lng,
      bounds._sw.lat,
    ];

    cityMarkers.forEach((marker) => {
      const city = marker.cityData;
      marker.setLngLat(city.coordinates);
    });
  }

  function addMarkers() {
    Object.keys(citiesYears).forEach((cityName) => {
      const city = citiesYears[cityName];
      const popup = new mapboxgl.Popup().setHTML(
        `<h3>${cityName}</h3><p>Year: ${city.year}</p>`
      );
      const marker = new mapboxgl.Marker()
        .setLngLat(city.coordinates)
        .addTo(map);

      marker.cityData = city;
      marker.setPopup(popup);

      marker.getElement().addEventListener('mouseenter', () => {
        marker.togglePopup();
      });

      marker.getElement().addEventListener('mouseleave', () => {
        marker.togglePopup();
      });
      cityMarkers.push(marker);
    });
  }

  
  let isVisible = false;
  $: if (index === 3) {
    isVisible = true;
  } else {
    isVisible = false;
  }


</script>

<svelte:head>
  <link
    rel="stylesheet"
    href="https://api.mapbox.com/mapbox-gl-js/v2.14.0/mapbox-gl.css"
  />
</svelte:head>

<div class="map"  class:visible={isVisible} bind:this={container} />

<style>
  .map {
    width: 90%;
    height: 70vh; /* check problem when setting width */
    position: absolute;
    opacity: 0;
    visibility: hidden;
    transition:
      opacity 2s,
      visibility 2s;
    outline: rgba(34, 33, 33, 0.053) solid 3px;
    /* padding: 1em; */
   margin: 0em 4em 4em 6em;
  }

  .map.visible {
    opacity: 1;
    visibility: visible;
  }
</style>
