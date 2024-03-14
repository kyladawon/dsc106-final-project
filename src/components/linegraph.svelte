<script>



import { onMount } from 'svelte';
import * as d3 from 'd3';
import Papa from 'papaparse';
import { scaleUtc, scaleLinear, extent, sum, timeParse} from 'd3';

let data = [];

async function convertCsvToJson() {
  const res = await fetch('olympic.csv');
  const csv = await res.text();

  const parsed = Papa.parse(csv, { header: true });
  const jsonData = { name: 'olympic', children: [] };

  parsed.data.forEach((entry) => {
    let hasNaN = false;
    for (let key in entry) {
      if ((entry[key] === 'NaN') || (entry[key] === '')) {
        hasNaN = true;
        break;
      }
    }
    if (!hasNaN) {
      jsonData.children.push({
        sport: entry.Sport,
        count: parseInt(entry.Medal) || 0 // Convert count to integer, default to 0 if parsing fails
      });
    }
  });

  return jsonData;
}

onMount(async () => {
    data = await convertCsvToJson(); // Await the JSON data
    createChart(data);
    console.log(data);
  });



  function createChart() {
    const width = 928;
  const height = 600;
  const marginTop = 20;
  const marginRight = 20;
  const marginBottom = 30;
  const marginLeft = 30;

  // Create the positional scales.
  const x = d3.scaleUtc()
    .domain(d3.extent(data, d => d.Year))
    .range([marginLeft, width - marginRight]);

    const totalMedals = sum(data, d => d.values);

  const y = scaleLinear()
    .domain([0, totalMedals])
    .range([height - marginBottom, marginTop]);

  // Create the SVG container.
  const svg = d3.create("svg")
      .attr("width", width)
      .attr("height", height)
      .attr("viewBox", [0, 0, width, height])
      .attr("style", "max-width: 100%; height: auto; overflow: visible; font: 10px sans-serif;");

  // Add the horizontal axis.
  svg.append("g")
      .attr("transform", `translate(0,${height - marginBottom})`)
      .call(d3.axisBottom(x).ticks(width / 80).tickSizeOuter(0));

  // Add the vertical axis.
  svg.append("g")
      .attr("transform", `translate(${marginLeft},0)`)
      .call(d3.axisLeft(y))
      .call(g => g.select(".domain").remove())
      .call(voronoi ? () => {} : g => g.selectAll(".tick line").clone()
          .attr("x2", width - marginLeft - marginRight)
          .attr("stroke-opacity", 0.1))
      .call(g => g.append("text")
          .attr("x", -marginLeft)
          .attr("y", 10)
          .attr("fill", "currentColor")
          .attr("text-anchor", "start")
          .text("↑ Unemployment (%)"));


const line = d3.line();
  const path = svg.append("g")
      .attr("fill", "none")
      .attr("stroke", "steelblue")
      .attr("stroke-width", 1.5)
      .attr("stroke-linejoin", "round")
      .attr("stroke-linecap", "round")
    .selectAll("path")
    .data(groups.values())
    .join("path")
      .style("mix-blend-mode", "multiply")
      .attr("d", line);
  }

  </script>