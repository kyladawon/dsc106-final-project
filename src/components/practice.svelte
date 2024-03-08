function updateCircles(currentYear) {
  const svg = d3.select('#circles');

  // Update circles based on the current year's data
  const circles = svg
    .selectAll('circle')
    .data(bubbleData[currentYear]);

  circles.enter()
    .append('circle')
    .merge(circles)
    .attr('r', (d) => Math.sqrt(d.val) / Math.PI)
    .attr('cx', (d) => d.x)
    .attr('cy', (d) => d.y)
    .attr('fill', (d) => d.color)
    .on('mouseover', function (event, d) {
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

  circles.exit().remove();
}
