<script>
  import { onMount } from 'svelte';
  import * as d3 from 'd3';
  import { bubbleData } from './bubbleData.js';

  const width = 500;
  const height = 500;

  let currentYear = 1976; // Initial year

  function updateCircles(currentYear) {
    const svg = d3.select('#circles');

    // Update circles based on the current year's data
    svg
      .selectAll('circle')
      .data(bubbleData[currentYear])
      .attr('r', (d) => Math.sqrt(d.val) / Math.PI)
      .attr('cx', (d) => d.x)
      .attr('cy', (d) => d.y)
      .attr('fill', (d) => d.color);

    // Update text labels based on the current year's data
    svg
      .selectAll('.source')
      .data(bubbleData[currentYear])
      .attr('x', (d) => d.x)
      .attr('y', (d) => d.y) // Positioned in the middle of the circle
      .text((d) => d.source)
      .style('font-family', 'arial')
      .style('font-size', '12px')
      .attr('text-anchor', 'middle')
      .attr('dy', '0.35em');

    // Add or update text labels for additional information
    const additionalInfo = svg
      .selectAll('.additional-info')
      .data(bubbleData[currentYear]);

    additionalInfo
      .enter()
      .append('text')
      .attr('class', 'additional-info')
      .attr('x', (d) => d.x)
      .attr('y', (d) => d.y + Math.sqrt(d.val) / Math.PI + 15) // Positioned below the circle
      .text((d) => `${currentYear}, ${d.val / 1000} Medals`)
      .style('font-family', 'arial')
      .style('font-size', '12px')
      .attr('text-anchor', 'middle')
      .attr('dy', '0.35em');

    additionalInfo
      .attr('x', (d) => d.x)
      .attr('y', (d) => d.y + Math.sqrt(d.val) / Math.PI + 15) // Positioned below the circle
      .text((d) => `${currentYear}, ${d.val / 1000} Medals`);
  }

  onMount(() => {
    const svg = d3.select('#circles');
    // Create circles initially
    svg
      .selectAll('circle')
      .data(bubbleData[currentYear])
      .enter()
      .append('circle')
      .attr('cx', (d) => d.x)
      .attr('cy', (d) => d.y)
      .attr('r', (d) => Math.sqrt(d.val) / Math.PI)
      .attr('fill', (d) => d.color);

    // Append text labels initially
    svg
      .selectAll('.source')
      .data(bubbleData[currentYear])
      .enter()
      .append('text')
      .attr('class', 'source')
      .attr('x', (d) => d.x)
      .attr('y', (d) => d.y) // Positioned in the middle of the circle
      .text((d) => d.source)
      .style('font-family', 'arial')
      .style('font-size', '12px')
      .attr('text-anchor', 'middle')
      .attr('dy', '0.35em');

    updateCircles(currentYear); // Initially update circles with the current year
  });
</script>

<svg style="width: 100%; height: 100%;">
  <g id="circles"></g>
  <g></g>
</svg>

<input
  type="range"
  min="1976"
  max="2008"
  step="4"
  bind:value={currentYear}
  on:input={() => updateCircles(currentYear)}
/>
