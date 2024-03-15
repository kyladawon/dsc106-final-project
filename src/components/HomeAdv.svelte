<script>
  import { onMount } from 'svelte';
  import * as d3 from 'd3';
  import {
    athens,
    barcelona,
    beijing,
    montreal,
    moscow,
    seoul,
    sydney,
    usa,
  } from './nineData.js';

  // Define an array of datasets
  const datasets = [
    { name: 'Athens', data: athens },
    { name: 'Barcelona', data: barcelona },
    { name: 'Beijing', data: beijing },
    { name: 'Montreal', data: montreal },
    { name: 'Moscow', data: moscow },
    { name: 'Seoul', data: seoul },
    { name: 'Sydney', data: sydney },
    { name: 'United States', data: usa },
  ];

  // Chart dimensions and margins
  const width = 928;
  const height = 500;
  const marginTop = 20;
  const marginRight = 30;
  const marginBottom = 30;
  const marginLeft = 40;

  // Selected dataset
  let selectedDataset = datasets[0];

  // Function to create scales
  function createScales(data) {
    const xScale = d3
      .scaleLinear()
      .domain(d3.extent(data, (d) => d.year))
      .range([marginLeft, width - marginRight]);

    const yScale = d3
      .scaleLinear()
      .domain([0, d3.max(data, (d) => d.count)])
      .range([height - marginBottom, marginTop]);

    return { xScale, yScale };
  }

  // Function to create line generator
  function createLineGenerator(xScale, yScale) {
    return d3
      .line()
      .x((d) => xScale(d.year))
      .y((d) => yScale(d.count));
  }

  // Function to update the line path with transition
  function updateLine() {
    const lineGenerator = createLineGenerator(
      createScales(selectedDataset.data).xScale,
      createScales(selectedDataset.data).yScale
    );

    // Select the line path and apply transition
    d3.select('.line')
      .transition()
      .duration(750) // Set the duration of the transition
      .attr('d', lineGenerator(selectedDataset.data));
  }

  // Call updateLine function on component mount
  onMount(updateLine);
</script>

<!-- Add the updateLine function call inside the select element -->
<select bind:value={selectedDataset} on:change={updateLine}>
  {#each datasets as dataset}
    <option value={dataset}>{dataset.name}</option>
  {/each}
</select>

<svg
  {width}
  {height}
  viewBox={'0 0 ' + width + ' ' + height}
  style="max-width: 100%; height: auto;"
>
  <g>
    <!-- X-Axis -->
    <line
      stroke="currentColor"
      x1={marginLeft - 6}
      x2={width}
      y1={height - marginBottom}
      y2={height - marginBottom}
    />

    {#each selectedDataset.data as point}
      <!-- X-Axis Ticks -->
      <line
        stroke="currentColor"
        x1={createScales(selectedDataset.data).xScale(point.year)}
        x2={createScales(selectedDataset.data).xScale(point.year)}
        y1={height - marginBottom}
        y2={height - marginBottom + 6}
      />

      <!-- X-Axis Tick Labels -->
      <text
        fill="currentColor"
        text-anchor="middle"
        x={createScales(selectedDataset.data).xScale(point.year)}
        y={height - marginBottom + 22}
      >
        {point.year}
      </text>
    {/each}
  </g>

  <g>
    <!-- Y-Axis and Grid Lines -->
    {#each createScales(selectedDataset.data).yScale.ticks() as tick}
      {#if tick !== 0}
        <!-- Grid Lines -->
        <line
          stroke="currentColor"
          stroke-opacity="0.1"
          x1={marginLeft}
          x2={width - marginRight}
          y1={createScales(selectedDataset.data).yScale(tick)}
          y2={createScales(selectedDataset.data).yScale(tick)}
        />

        <!-- Y-Axis Ticks -->
        <line
          stroke="currentColor"
          x1={marginLeft - 6}
          x2={marginLeft}
          y1={createScales(selectedDataset.data).yScale(tick)}
          y2={createScales(selectedDataset.data).yScale(tick)}
        />
      {/if}

      <!-- Y-Axis Tick Labels -->
      <text
        fill="currentColor"
        text-anchor="end"
        dominant-baseline="middle"
        x={marginLeft - 9}
        y={createScales(selectedDataset.data).yScale(tick)}
      >
        {tick}
      </text>
    {/each}

    <!-- Y-Axis Label -->
    <text fill="currentColor" text-anchor="start" x={-marginLeft} y={15}>
      ↑ Count
    </text>
  </g>

  <!-- Define the path element -->
  <path class="line" fill="none" stroke="steelblue" stroke-width="1.5" />

  <!-- Highlight the point corresponding to the city name -->
  <circle
    cx={createScales(selectedDataset.data).xScale(
      selectedDataset.data.find((d) => d.city === selectedDataset.name).year
    )}
    cy={createScales(selectedDataset.data).yScale(
      selectedDataset.data.find((d) => d.city === selectedDataset.name).count
    )}
    r="5"
    fill="red"
  />

  {#each selectedDataset.data as point}
    <g>
      <text
        x={createScales(selectedDataset.data).xScale(point.year)}
        y={createScales(selectedDataset.data).yScale(point.count) - 10}
        fill="black"
        text-anchor="middle"
        dominant-baseline="baseline"
      >
        {point.count}
      </text>
    </g>
  {/each}
</svg>
