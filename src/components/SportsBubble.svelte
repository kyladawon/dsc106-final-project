<script>
  import { onMount } from 'svelte';
  import * as d3 from 'd3';
  import { bubbleData } from './bubbleData.js';

  const width = 500;
  const height = 500;

  let data = bubbleData;
  let currentYear = 1976; // Initial year

  function updateCircles(currentYear) {
    console.log(currentYear);
    console.log(data[currentYear]);
    const svg = d3.select('#circles');
    console.log(svg);
    // Clear previous content
    //d3.selectAll("*").remove();
    // Append circles
    svg
      .selectAll()
      .data(data[currentYear], (d) => {
        console.log(d);
        return d.source;
      })
      .join('circle')
      .attr('cx', (d) => d.x)
      .attr('cy', (d) => d.y)
      .attr('r', (d) => {
        // console.log(d);
        return Math.sqrt(d.val) / Math.PI;
      })
      .attr('fill', (d) => d.color)
      .on('mouseover', function (event, d) {
        console.log(d.x);
        console.log(d.y);
        console.log(d.val);
        console.log(d.color);
        // Show tooltip
        const tooltip = document.querySelector('.tooltip');
        let tooltipContent = `
              ${d.source}<br>
              Year: ${currentYear}<br>
              ${d.val / 1000} Medals`;
        tooltip.innerHTML = tooltipContent;
        tooltip.style.opacity = 1;
      })
      .on('mousemove', function (event) {
        // Position tooltip relative to the mouse cursor
        const tooltip = document.querySelector('.tooltip');
        tooltip.style.left = event.pageX + 'px';
        tooltip.style.top = event.pageY + 'px';
      })
      .on('mouseleave', function () {
        const tooltip = document.querySelector('.tooltip');
        tooltip.style.opacity = 0;
      });

    //Append text labels
    svg
      .selectAll('text')
      .data(data[currentYear])
      .enter()
      .append('text')
      .attr('x', (d) => d.x)
      .attr('y', (d) => d.y)
      .text((d) => d.source)
      .style('font-family', 'arial')
      .style('font-size', '12px')
      .attr('text-anchor', 'middle')
      .attr('dy', '0.35em');
  }

  onMount(() => {
    updateCircles(currentYear); // Initially update circles with the current year
  });
</script>

<div class="tooltip" style="opacity: 0;">
  <div class="year" style="opacity: 0;"></div>
</div>

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
  on:change={() => updateCircles(currentYear)}
/>

<style>
  .tooltip {
    position: absolute;
    background-color: white;
    border: 1px solid #aaa;
    padding: 10px;
    pointer-events: none;
    z-index: 999;
  }
</style>
