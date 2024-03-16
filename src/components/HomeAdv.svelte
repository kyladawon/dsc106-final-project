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
  const width = 628;
  const height = 400;
  const marginTop = 10;
  const marginRight = 30;
  const marginBottom = 50;
  const marginLeft = 80;

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

  if (selectedDataset.name == 'usa') {
    updateUSACircleAndText();
  }

  function updateUSACircleAndText() {
    const usaTotalCount = usa.reduce((total, city) => total + city.count, 0);

    // Update red circle
    const redCircle = d3.select('.usa-circle');
    redCircle.attr('cx', createScales(usa).xScale(usa[0].year)); // Assuming there's only one year data for "USA"
    redCircle.attr('cy', createScales(usa).yScale(usaTotalCount)); // Using the total count for "USA"

    // Update associated text
    const text = d3.select('.usa-count');
    text.text(usaTotalCount); // Displaying the total count for "USA"
    text.attr('x', createScales(usa).xScale(usa[0].year)); // Assuming there's only one year data for "USA"
    text.attr('y', createScales(usa).yScale(usaTotalCount) - 10); // Adjusting the position of the text
  }
  // Call updateLine function on component mount
  onMount(updateLine);
</script>

<div style="margin-top: 80px;"> <!-- Add margin-top to move the button down -->
  <select class = 'selection-button' bind:value={selectedDataset} on:change={updateLine}>
    {#each datasets as dataset}
      <option value={dataset}>{dataset.name}</option>
    {/each}
  </select>
</div>
<br />

<svg
  {width}
  {height}
  viewBox={'0 0 ' + width + ' ' + height}
  style="max-width: 100%; height: auto;"
>
  <g>
    <line
      stroke="currentColor"
      x1={marginLeft - 6}
      x2={width}
      y1={height - marginBottom}
      y2={height - marginBottom}
    />

    {#each selectedDataset.data as point}
      <line
        stroke="currentColor"
        x1={createScales(selectedDataset.data).xScale(point.year)}
        x2={createScales(selectedDataset.data).xScale(point.year)}
        y1={height - marginBottom}
        y2={height - marginBottom + 6}
      />

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
    {#each createScales(selectedDataset.data).yScale.ticks() as tick}
      {#if tick !== 0}
        <line
          stroke="currentColor"
          stroke-opacity="0.1"
          x1={marginLeft}
          x2={width - marginRight}
          y1={createScales(selectedDataset.data).yScale(tick)}
          y2={createScales(selectedDataset.data).yScale(tick)}
        />

        <line
          stroke="currentColor"
          x1={marginLeft - 6}
          x2={marginLeft}
          y1={createScales(selectedDataset.data).yScale(tick)}
          y2={createScales(selectedDataset.data).yScale(tick)}
        />
      {/if}

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

    <text
    fill="purple"
    transform="rotate(-90)"
    text-anchor="middle"
    x={-marginLeft - 130} 
    y={height / 10}
  >
    Total Medal Count
  </text>

    <text fill="purple" text-anchor="middle" x={width / 2 + 25} y={height - 5}>
      Year
    </text>

  </g>

  <path class="line" fill="none" stroke="steelblue" stroke-width="1.5" />

  {#each selectedDataset.data as point}
    {#if selectedDataset.name === 'United States' && (point.city === 'Los Angeles' || point.city === 'Atlanta')}
      <circle
        cx={createScales(selectedDataset.data).xScale(point.year)}
        cy={createScales(selectedDataset.data).yScale(point.count)}
        r="5"
        fill="red"
      />
    {:else if point.city === selectedDataset.name}
      <circle
        cx={createScales(selectedDataset.data).xScale(point.year)}
        cy={createScales(selectedDataset.data).yScale(point.count)}
        r="5"
        fill="red"
      />
    {/if}
  {/each}

  {#each selectedDataset.data as point}
    <g>
      <text
        class="usa-count"
        fill="black"
        text-anchor="middle"
        dominant-baseline="baseline"
      ></text>
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

<style>
  svg {
    margin-top: 0px;
  }
  text {
    font-size: 20px;
  }

  .selection-button {
  
  border: 1px solid #ccc; /* Add border */
  border-radius: 5px; /* Add border radius */
  background-color: #f0f0f0; /* Add background color */
  color: #333; /* Add text color */
  cursor: pointer; /* Change cursor on hover */
}

</style>

