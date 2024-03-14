<script>
  import Scroller from '@sveltejs/svelte-scroller';
  import Map from './Map.svelte';
  import { geoMercator } from 'd3-geo';
  import Project3 from './Proj3.svelte';
  // import Chart from './Chart.svelte';
  import Athlete from './Athlete.svelte';
  import { tweened } from 'svelte/motion';
  import { cubicOut } from 'svelte/easing';
  import Video from './Video.svelte';
  import Bubble from './SportsBubble.svelte';
  import Linegraph from './linegraph.svelte';
  import Country from './country.svelte';
    import SportsBubble from './SportsBubble.svelte';


  let count, index, offset, progress;
  let width, height;

  let geoJsonToFit = {
    type: 'FeatureCollection',
    features: [
      {
        type: 'Feature',
        geometry: {
          type: 'Point',
          coordinates: [1, 0],
        },
      },
      {
        type: 'Feature',
        geometry: {
          type: 'Point',
          coordinates: [0, 1],
        },
      },
    ],
  };

  $: projection = geoMercator().fitSize([width, height], geoJsonToFit);

  let isHovered = false;

  function handleMouseOver() {
    isHovered = true;
  }

  function handleMouseOut() {
    isHovered = false;
  }
</script>



<div class="left">
 
    <section class="s1-left">
      {#if index === 0}
        <Project3 />
      {/if}
    </section>
    <section class="s2-left">
      {#if index === 1}
        <Country />
      {/if}
    </section>
    <section class="s3-left">
      {#if index === 2}
        <Map />
      {/if}
    </section>
    <section class="s4-left">
      {#if index === 3}
        <SportsBubble />
      {/if}
    </section>
    <section class="s5-left">
      {#if index === 4}
        <Athlete />
      {/if}
    </section>
    <section class="s6-left">
      {#if index === 5}
        <Linegraph />
      {/if}
    </section>
    <section class="s7-left">
    </section>
</div>


<div class="right">
  <Scroller
    top={0.0}
    bottom={1}
    threshold={0.5}
    bind:count
    bind:index
    bind:offset
    bind:progress
  >
  <div slot="foreground">
    <!-- <section class="centered-content"><br><br><br></section> -->
    <section class="s1">
      <p>section 1</p>
    </section>
    <section class="s2">
      <p>section 2 </p>
    </section>
    <section class="s3">
      <p>section 3</p>
    </section>
    <section class="s4">
      <p> section 4 </p>
    </section>
    <section class="s4">
      <p> section 5 </p>
    </section>
    <section class="s4">
      <p> section 6 </p>
    </section>
    <section class="s4">
      <p> section 7 </p>
    </section>
  </div>
</Scroller>
</div>


<style>

.left {
  left:0;
  top:0;
  position:fixed;
  height:100vh;
  width: 65%;
 }
 .right {
    position: absolute;
    right: 0;
    top:60vh;
    width: 36%;
    margin-right: -1vw;
    background-color: #ffffff80;
  }

  .s1-left {
    background-color: #e9d28cc8;
  }

  .s2-left {
    background-color: #aaeb5f80;
  }

  .s3-left {
    background-color: #f2b5df80;
  }

  .s4-left {
    background-color: #a0aff180;
  }

  .s1 {
    min-height: 110vh;
    color: black;
    background-color: #e9d28cc8;
    
    background-position: center;
  }
 
  .s2 {
    min-height: 110vh;
    color: black;
    background-color: #aaeb5f80;
    background-position: center;
  }
  .s3 {
    min-height: 110vh;
    color: black;
    background-color: #f2b5df80;
    background-position: center;
  }
  .s4 {
    min-height: 110vh;
    color: black;
    background-color: #a0aff180;
    background-position: center;
  }
 
  
 
</style>


 

  



  <!-- /* .progress-bars {
    position: absolute;
    background: rgba(170, 51, 120, 0.2) /*  40% opaque */ -->
  <!-- /* visibility: visible; */
  /* }  */

  /* .banner {
    background-image: linear-gradient(
      to bottom right,
      rgba(0, 129, 200, 0.5),
      rgba(252, 177, 49, 0.5),
      rgba(0, 166, 81, 0.5),
      rgba(238, 51, 78, 0.5)
    );
    padding: 20px;
    margin-bottom: 20px;
    border-radius: 10px;
    color: black;
  } */ -->

  

 <!-- ORIGINAL SCROLLER AND STYLE -->

 <!-- <Scroller
 top={0.0}
 bottom={1}
 threshold={0.5}
 bind:count
 bind:index
 bind:offset
 bind:progress
>
 <div
   class="background"
   slot="background"
   bind:clientWidth={width}
   bind:clientHeight={height}
 >
   <section class = "b1"></section>
 </div>

 <section class = "section1">
  <div class="banner">
    <h1 class="title">United States in the Olympics</h1>
    <p> Kyla Park and Essie Cheng </p>
</div>
    <h2 class = "hook">
      <i>Nothing Lasts Forever </i>
    </h2>
    <p class = hook2> 
      Explore Olympic excellence and dive into the intricate dynamics of the 
United States' Olympic dominance, where a combination of home advantage 
and exceptional performances in certain sports and athletes have 
fueled a legacy of success. But..
    </p>
    <p class = hook3>
      Shifting landscapes and emerging talents are bound to reshape the
      narrative of the U.S. in the Olympics. As the trajectory evolves, we
      must discover potential areas for future investment and growth.
    </p>
    <div class="olympic-logo"></div>
  </section>


<section class="section2">
  Data Exploration
  <Project3 />
</section>
<section class="section3">
  Where have the Olympics Been Held?
  <Map bind:geoJsonToFit {index} />
</section>

<section class="section4">
  U.S. Best Performing Sports
  <Bubble />
</section>

<section class = "section6">
  <Athlete />
</section>

<section class = "section5">
  <Country />

</section>
<section class = "section7">
  
  <Video />
</section>
<section>U.S. Gender Based Performance</section>
<section class = "section8">Is there a home field advantage?</section>
<section class = "section9">Closing Thoughts
  
  <section id="custom-background-section">
    <h3 class = "takeaway"> 
      The U.S. has been dominant in the Olympics thanks to excelling in
      <b>aquatics</b> and <b>athletics</b>, with the top Olympic athletes 
      of all time earning their medals in those disciplines. However, <i>
      change is inevitable. </i> Decline in medals won can be seen in many
      of the U.S.'s top sports, and other countries are bound to catch up. 
      However, improvement can be seen in <b> fencing, shooting, cycling, 
      and taekwondo </b>. Alongside these rising sports are rising athletes,
      consistently helping drive success in these sports. Therefore, the U.S.
      should focus more on future investment and growth in <b> fencing, shooting, 
      cycling, and taekwondo </b> in order to sustain the U.S.'s dominance in the 
      Olympics.
    </h3>
  </section>

</section>
</div>

</Scroller>




<style>

.olympic-logo {
width: 120px; /* Adjust size as needed */
height: 1200px; /* Adjust size as needed */
background-image: url('logo.png'); /* Replace with the URL of your Olympic logo */
background-size: contain;
background-repeat: no-repeat;
position: relative;
bottom: 75px;
left: -100px; /* Initial position outside the screen */
animation: moveLogo 10s linear infinite; /* Adjust duration as needed */
}

@keyframes moveLogo {
from {
    left: -100px; /* Start position */
}
to {
    left: calc(100% + 100px); /* End position (right edge of the screen plus logo width) */
}
}
.background {
width: 100%;
height: 100vh;
position: relative;
/* outline: green solid 3px; */
font-family: 'Times New Roman';
}
.banner {
background-color: rgba(0, 0, 0, 0.7); /* Background color */
padding: 20px; /* Padding */
margin-bottom: 20px; /* Bottom margin to separate from other content */
text-align: center; /* Center align text */
}
.banner p {
color: white;
}
.title {
font-family: 'Times New Roman';
font-size: 100px; 
font-weight: bold;
color: white;
text-shadow: 0px 0px 5px #30DCE5, 0px 0px 10px #30DCE5, 0px 0px 10px #30DCE5,
0px 0px 20px #b393d3;
}
.hook {
font-family: 'Times New Roman';
font-size: 50px; 
font-weight: bold; 
color: white;
text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
line-height: 1.5;
position: relative;
top: 100px;
background-color: rgba(0, 0, 0, 0.7); 
padding: 10px 20px; 
display: inline-block; 
border: 2px solid #30DCE5;
}

.hook2 {
font-family: 'Times New Roman';
line-height: 1.5;
position: relative;
top: -200px;
color: white;
background-color: rgba(0, 0, 0, 0.7); 
padding: 20px; 
border: 2px solid #30DCE5;
box-shadow: 0 0 10px #30DCE5;
display: inline-block; 

}

.hook3 {
font-family: 'Times New Roman';
line-height: 1.5;
position: relative;
top: -70px;
color: white;
background-color: rgba(0, 0, 0, 0.7); 
padding: 20px; 
border: 2px solid #30DCE5;
box-shadow: 0 0 10px #30DCE5; 
display: inline-block; 

}

.foreground {
width: 100%;
margin: 0 auto;
height: auto;
position: relative;

}
section {
height: 100vh;
background-color: rgba(0, 0, 0, .1); 
color: white;

text-align: center;
max-width: 100%; 
color: black;

font-family: 'times new roman';
text-align: center;
font-size: 35px;
line-height: 28px;
font-weight: bold;
padding-top: 10px;

}
.section1{
font-size: 20px;
background-image: url('b.jpeg');
 background-size: cover; 
background-position: center; 
}
.section2 {
background-image: linear-gradient(
  to bottom right,
  rgba(0, 255, 0, 0.5),
  rgba(0, 100, 0, 0.5)
);
}

.section3 {
background-image: linear-gradient(
  to bottom right,
  rgba(255, 255, 0, 0.5),
  rgba(255, 0, 0, 0.5)
);
}

.section4 {
background-image: linear-gradient(
  to bottom right,
  rgba(0, 0, 255, 0.5),
  rgba(0, 255, 255, 0.5)
);
}
.section5 {
background-image: linear-gradient(
  to bottom right,
  rgba(128, 0, 128, 0.5),
rgba(255, 192, 203, 0.5)
);
}
.section6 {
background-image: linear-gradient(
  to bottom right,
  rgba(0, 0, 128, 0.5),
rgba(0, 0, 0, 0.5)
);
}
.section7 {
background-image: linear-gradient(
  to bottom right,
  rgba(0, 0, 128, 0.5),
rgba(0, 0, 0, 0.5)
);
}

.takeaway {
color: white;
line-height: 1.5;
background-color: rgba(0, 0, 0, .7); 
padding: 5px 10px;
}


#custom-background-section {
  position: relative; 
  height: 100%; 
  overflow: hidden; 
}

#custom-background-section::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-size: cover; 
  background-position: center; 
  opacity: 0.5;
  z-index: -1; 
  animation: switchBackground 15s infinite; 

@keyframes switchBackground {
          0%, 20% {
              background-image: url("olympics4.jpeg");
          }
          45%,55% {
              background-image: url("olympics5.jpeg");
          }
          55%,70% {
              background-image: url("olympics6.jpeg");
          }
          100%,0% {
              background-image: url('olympics7.jpeg');
          }
      }
</style>
 -->
