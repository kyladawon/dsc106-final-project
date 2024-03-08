<script>
  import { onMount } from 'svelte';
  import * as d3 from 'd3';

  const width = 500;
  const height = 500;

  // Data for different years
  const data = {
    1976: [
      { source: 'Aquatics', x: 250, y: 300, val: 51 * 1000, color: '#1897EE' },
      { source: 'Athletics', x: 450, y: 300, val: 31 * 1000, color: '#F8BF6C' },
      {
        source: 'Basketball',
        x: 650,
        y: 300,
        val: 24 * 1000,
        color: '#E3E1B2',
      },
      { source: 'Rowing', x: 850, y: 300, val: 12 * 1000, color: '#F9CAC8' },
      {
        source: 'Equestrian',
        x: 1050,
        y: 300,
        val: 9 * 1000,
        color: '#D1C2E0',
      },
    ],
    1980: [],
    1984: [
      { source: 'Aquatics', x: 250, y: 300, val: 73 * 1000, color: '#1897EE' },
      { source: 'Athletics', x: 450, y: 300, val: 52 * 1000, color: '#F8BF6C' },
      { source: 'Rowing', x: 650, y: 300, val: 38 * 1000, color: '#E3E1B2' },
      {
        source: 'Gymnastics',
        x: 850,
        y: 300,
        val: 26 * 1000,
        color: '#F9CAC8',
      },
      {
        source: 'Basketball',
        x: 1050,
        y: 300,
        val: 24 * 1000,
        color: '#D1C2E0',
      },
    ],
    1988: [
      { source: 'Aquatics', x: 250, y: 300, val: 57 * 1000, color: '#1897EE' },
      { source: 'Athletics', x: 450, y: 300, val: 35 * 1000, color: '#F8BF6C' },
      {
        source: 'Basketball',
        x: 650,
        y: 300,
        val: 24 * 1000,
        color: '#E3E1B2',
      },
      { source: 'Rowing', x: 850, y: 300, val: 14 * 1000, color: '#F9CAC8' },
      {
        source: 'Volleyball',
        x: 1050,
        y: 300,
        val: 12 * 1000,
        color: '#D1C2E0',
      },
    ],
    1992: [
      { source: 'Aquatics', x: 250, y: 300, val: 62 * 1000, color: '#1897EE' },
      { source: 'Athletics', x: 450, y: 300, val: 48 * 1000, color: '#F8BF6C' },
      {
        source: 'Basketball',
        x: 650,
        y: 300,
        val: 24 * 1000,
        color: '#E3E1B2',
      },
      {
        source: 'Volleyball',
        x: 850,
        y: 300,
        val: 24 * 1000,
        color: '#F9CAC8',
      },
      { source: 'Sailing', x: 1050, y: 300, val: 16 * 1000, color: '#D1C2E0' },
    ],
    1996: [
      { source: 'Aquatics', x: 250, y: 300, val: 72 * 1000, color: '#1897EE' },
      { source: 'Athletics', x: 450, y: 300, val: 39 * 1000, color: '#F8BF6C' },
      {
        source: 'Basketball',
        x: 650,
        y: 300,
        val: 24 * 1000,
        color: '#E3E1B2',
      },
      { source: 'Baseball', x: 850, y: 300, val: 20 * 1000, color: '#F9CAC8' },
      { source: 'Football', x: 1050, y: 300, val: 16 * 1000, color: '#D1C2E0' },
    ],
    2000: [
      { source: 'Aquatics', x: 250, y: 300, val: 80 * 1000, color: '#1897EE' },
      { source: 'Athletics', x: 450, y: 300, val: 27 * 1000, color: '#F8BF6C' },
      { source: 'Baseball', x: 650, y: 300, val: 24 * 1000, color: '#E3E1B2' },
      {
        source: 'Basketball',
        x: 850,
        y: 300,
        val: 24 * 1000,
        color: '#F9CAC8',
      },
      { source: 'Football', x: 1050, y: 300, val: 18 * 1000, color: '#D1C2E0' },
    ],
    2004: [
      { source: 'Aquatics', x: 250, y: 300, val: 89 * 1000, color: '#1897EE' },
      { source: 'Athletics', x: 450, y: 300, val: 38 * 1000, color: '#F8BF6C' },
      {
        source: 'Basketball',
        x: 650,
        y: 300,
        val: 24 * 1000,
        color: '#E3E1B2',
      },
      {
        source: 'Gymnastics',
        x: 850,
        y: 300,
        val: 19 * 1000,
        color: '#F9CAC8',
      },
      { source: 'Rowing', x: 1050, y: 300, val: 18 * 1000, color: '#D1C2E0' },
    ],
    2008: [
      { source: 'Aquatics', x: 250, y: 300, val: 94 * 1000, color: '#1897EE' },
      { source: 'Athletics', x: 450, y: 300, val: 29 * 1000, color: '#F8BF6C' },
      {
        source: 'Volleyball',
        x: 650,
        y: 300,
        val: 28 * 1000,
        color: '#E3E1B2',
      },
      { source: 'Baseball', x: 850, y: 300, val: 24 * 1000, color: '#F9CAC8' },
      {
        source: 'Basketball',
        x: 1050,
        y: 300,
        val: 24 * 1000,
        color: '#D1C2E0',
      },
    ],
  };

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
