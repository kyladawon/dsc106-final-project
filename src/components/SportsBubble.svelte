<script>
  import { onMount } from 'svelte';
  import * as d3 from 'd3';
  import { bubbleData } from './bubbleData.js';

  const width = 500;
  const height = 500;

  let currentYear = 1976;
  let isPlaying = false;
  let animationTimer;

  const annotations = [
    // { year: 1976, annotations: [
    //   { text: "Annotation 1 for 1976", x: 50, y: 50 },
    //   { text: "Annotation 2 for 1976", x: 100, y: 100 }
    // ]},
    {
      year: 1980,
      annotations: [
        { text: 'The U.S. boycotted the Olympics this year!', x: 650, y: 300 },
      ],
    },
    {
      year: 1984,
      annotations: [
        {
          text: '16-year-old Mary Lou Retton won the all-around individual gold for the first time in history',
          x: 775,
          y: 125,
        },
        {
          text: 'Carl Lewis emerged as the American men’s star, equalling Jesse Owens’ 1936 feat of winning four gold medals in track & field',
          x: 372,
          y: 65,
        },
      ],
    },
    {
      year: 1988,
      annotations: [
        {
          text: 'Florence Griffith Joyner , winning three gold medals and one silver',
          x: 375,
          y: 150,
        },
      ],
    },
    {
      year: 1992,
      annotations: [
        {
          text: 'This was the first year the USA was allowed to use professional players from the NBA',
          x: 570,
          y: 135,
        },
      ],
    },
    {
      year: 1996,
      annotations: [
        {
          text: 'The biggest medal winner was swimmer Amy Van Dyken, who won four gold medals',
          x: 170,
          y: 100,
        },
      ],
    },
    {
      year: 2000,
      annotations: [
        {
          text: 'Marion Jones won five track medals, the most ever won in track & field at a single Olympics by a woman',
          x: 370,
          y: 100,
        },
      ],
    },
    // { year: 2004, annotations: [
    //   { text: "Annotation 1 for 1984", x: 150, y: 150 },
    //   { text: "Annotation 2 for 1984", x: 200, y: 200 }
    // ]},
    {
      year: 2008,
      annotations: [
        {
          text: 'Swimmer Michael Phelps became the most successful man at an Olympics by winning eight gold medals',
          x: 175,
          y: 60,
        },
      ],
    },
  ];

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
      .style('font-family', 'times new roman')
      .style('font-size', '14px')
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
      .style('font-family', 'times new roman')
      .style('font-size', '16px')
      .attr('text-anchor', 'middle')
      .attr('dy', '0.35em');

    additionalInfo
      .attr('x', (d) => d.x)
      .attr('y', (d) => d.y + Math.sqrt(d.val) / Math.PI + 15) // Positioned below the circle
      .text((d) => `${currentYear}, ${d.val / 1000} Medals`);
  }

  // function addAnnotations(year) {
  //   const svg = d3.select('#circles');
  //   annotations.forEach((annotation) => {
  //     if (annotation.year === year) {
  //       annotation.annotations.forEach((a) => {
  //         const { text, x, y } = a;
  //         const annotationGroup = svg.append('g').attr('class', 'annotation');
  //         annotationGroup
  //           // .append('line')
  //           .attr('x1', x)
  //           .attr('y1', y)
  //           .attr('x2', x)
  //           .attr('y2', y - 50)
  //           .attr('stroke', 'black');
  //         annotationGroup
  //           .append('text')
  //           .attr('x', x)
  //           .attr('y', y - 60)
  //           .text(text)
  //           .style('font-family', 'arial')
  //           .style('font-size', '12px')
  //           .attr('text-anchor', 'middle')
  //           .attr('dy', '0.35em');
  //       });
  //     }
  //   });
  // }

  function addAnnotations(year) {
  const svg = d3.select('#circles');
  annotations.forEach((annotation) => {
    if (annotation.year === year) {
      annotation.annotations.forEach((a) => {
        const { text, x, y } = a;
        const annotationGroup = svg.append('g').attr('class', 'annotation');
        // annotationGroup
        //   .append('line')
        //   .attr('x1', x)
        //   .attr('y1', y)
        //   .attr('x2', x)
        //   .attr('y2', y - 50)
        //   .attr('stroke', 'black');
        // Use foreignObject to create multiline text
        const foreignObject = annotationGroup
          .append('foreignObject')
          .attr('x', x)
          .attr('y', y)
          .attr('width', 160) // adjust width as needed
          .attr('height', 200); // adjust height as needed
        foreignObject
          .append('xhtml:div')
          .style('font-family', 'times new roman')
          .style('font-size', '14px')
          .style('text-anchor', 'middle')
          //.style('color', 'white')
          .html(text); // Insert HTML content for multiline text
      });
    }
  });
}


  function startAnimation() {
    if (!isPlaying) {
      isPlaying = true;
      animationTimer = setInterval(() => {
        currentYear += 4; // Increment year by 4 for every step
        if (currentYear > 2008) {
          currentYear = 1976; // Reset to the initial year if it reaches the end
        }
        updateCircles(currentYear);
        updateLabel(currentYear);
        d3.selectAll('.annotation').remove();
        addAnnotations(currentYear);
      }, 1500); // Set interval to 1 second
    }
  }

  function stopAnimation() {
    isPlaying = false;
    clearInterval(animationTimer);
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

    addAnnotations(currentYear);
  });

  let label = '1976';
  function updateLabel(year) {
    label = year.toString();
  }
</script>

<svg style="width: 100%; height: 100%;">
  <g id="circles"></g>
  <g id="legend" transform="translate(1200,15)">
    <circle cx="5" cy="5" r="8" fill="#266DE1"></circle>
    <text x="15" y="10" style="font-family: times new roman; font-size: 15px;"
      >Aquatics</text
    >
    <circle cx="5" cy="25" r="8" fill="#2CC852"></circle>
    <text x="15" y="30" style="font-family: times new roman; font-size: 15px;"
      >Athletics</text
    >
    <circle cx="5" cy="45" r="8" fill="#E65E7B"></circle>
    <text x="15" y="50" style="font-family: times new roman; font-size: 15px;"
      >Baseball</text
    >
    <circle cx="5" cy="65" r="8" fill="#F5AE47"></circle>
    <text x="15" y="70" style="font-family: times new roman; font-size: 15px;"
      >Basketball</text
    >
    <circle cx="5" cy="85" r="8" fill="#D1C2E0"></circle>
    <text x="15" y="90" style="font-family: times new roman; font-size: 15px;"
      >Equestrian</text
    >
    <circle cx="5" cy="105" r="8" fill="#E8E525"></circle>
    <text x="15" y="110" style="font-family: times new roman; font-size: 15px;"
      >Football</text
    >
    <circle cx="5" cy="125" r="8" fill="#9947F5"></circle>
    <text x="15" y="130" style="font-family: times new roman; font-size: 15px;"
      >Gymnastics</text
    >
    <circle cx="5" cy="145" r="8" fill="#F9CAC8"></circle>
    <text x="15" y="150" style="font-family: times new roman; font-size: 15px;"
      >Rowing</text
    >
    <circle cx="5" cy="165" r="8" fill="#BBE3E1"></circle>
    <text x="15" y="170" style="font-family: times new roman; font-size: 15px;"
      >Sailing</text
    >
    <circle cx="5" cy="185" r="8" fill="#D8E3BB"></circle>
    <text x="15" y="190" style="font-family: times new roman; font-size: 15px;"
      >Volleyball</text
    >
    <!-- Add more rectangles and text labels as needed -->
  </g>
</svg>

<div style="position: relative; margin-top: -350px;">
  <div style="position: absolute; left: 40%;">
    <span>{label}</span>
    <input
      type="range"
      min="1976"
      max="2008"
      step="4"
      bind:value={currentYear}
      on:input={() => {
        updateCircles(currentYear);
        updateLabel(currentYear);
        // Remove existing annotations before adding new ones
        d3.selectAll('.annotation').remove();
        addAnnotations(currentYear);
      }}
    />

    <button
    style="padding: 5px 15px; background-color: black; color: white; border: none; border-radius: 6px; cursor: pointer;"
      on:click={() => {
        isPlaying ? stopAnimation() : startAnimation();
      }}
    >
      {isPlaying ? 'Stop' : 'Play'}
    </button>
  </div>
</div>


