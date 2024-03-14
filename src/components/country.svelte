<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';


    let points = [];
    const spacing = 12;
    let radius = 4;
    let pointsPerSideX;
    let totalGeneratedPoints;
    let data = [];


    async function fetchData() {
        const resForward = await fetch('olympic.csv');
        const csvForward = await resForward.text();
        data = d3.csvParse(csvForward, d3.autoType);
        generatePoints();
    }

    onMount(() => {
        fetchData();
    });

    function generatePoints() {
        points = [];
        
        const filteredData = data.filter(d => d.Country_Code === 'USA' || d.Country_Code === 'URS' || d.Country_Code === 'AUS');

        // Count medals for each country
        const medalCounts = {
            USA: 0,
            URS: 0,
            AUS: 0
        };

        filteredData.forEach(d => {
            if (d.Country_Code === 'USA') medalCounts.USA++;
            else if (d.Country_Code === 'URS') medalCounts.URS++;
            else if (d.Country_Code === 'AUS') medalCounts.AUS++;
        });

        // Calculate the total number of generated points
        totalGeneratedPoints = medalCounts.USA + medalCounts.URS + medalCounts.AUS;

        // Calculate pointsPerSideX dynamically based on the total number of points
        pointsPerSideX = Math.ceil(Math.sqrt(totalGeneratedPoints));

        // Generate points for each country based on the medal count
        let currentX = 0;
        let currentY = 0;

        // USA points
        for (let i = 0; i < medalCounts.USA; i++) {
            points.push({
                x: currentX * spacing + radius,
                y: currentY * spacing + radius,
                country: 'USA'
            });

            currentX++;
            if (currentX >= pointsPerSideX) {
                currentX = 0;
                currentY++;
            }
        }

        // SOV points
        for (let i = 0; i < medalCounts.URS; i++) {
            points.push({
                x: currentX * spacing + radius,
                y: currentY * spacing + radius,
                country: 'URS'
            });

            currentX++;
            if (currentX >= pointsPerSideX) {
                currentX = 0;
                currentY++;
            }
        }

        // AUS points
        for (let i = 0; i < medalCounts.AUS; i++) {
            points.push({
                x: currentX * spacing + radius,
                y: currentY * spacing + radius,
                country: 'AUS'
            });

            currentX++;
            if (currentX >= pointsPerSideX) {
                currentX = 0;
                currentY++;
            }
        }
    }

</script>




<svg width="800" height="800">
    {#each points as point, i}
        <circle 
        cx={point.x} 
        cy={point.y} 
        r={radius} 
        fill={point.country === 'USA' ? '#DEC70D' : (point.country === 'URS' ? '#A5A1A1' : '#C67C03')} 
      
        />
    {/each}

   
</svg>
