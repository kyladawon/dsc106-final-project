<script>
  import Scroller from '@sveltejs/svelte-scroller';
  import Map from './Map.svelte';
  import { geoMercator } from 'd3-geo';
  import Project3 from './Proj3.svelte';
  import Athlete from './Athlete.svelte';
  import { tweened } from 'svelte/motion';
  import { cubicOut } from 'svelte/easing';
  import Bubble from './SportsBubble.svelte';
  import Country from './country.svelte';
  import HomeAdv from './HomeAdv.svelte';

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

<Scroller
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
    <!-- <Graph {index} {width} {height} {projection} /> -->
    <!-- <div class="progress-bars">
      <p>current section: <strong>{index + 1}/{count}</strong></p>
      <progress value={count ? (index + 1) / count : 0} />
 
 
      <p>offset in current section</p>
      <progress value={offset || 0} />
 
 
      <p>total progress</p>
      <progress value={progress || 0} />
    </div> -->
    <section class="b1"></section>
  </div>

  <div class="foreground" slot="foreground">
    <section class="section1">
      <div class="banner">
        <h1 class="title">United States in the Olympics</h1>
        <p>Kyla Park and Essie Cheng</p>
      </div>
      <h2 class="hook">
        <i>Nothing Lasts Forever </i>
      </h2>
      <p class="hook2">
        Explore Olympic excellence and dive into the intricate dynamics of the
        United States' Olympic dominance, where a combination of home advantage
        and exceptional performances in certain sports and athletes have fueled
        a legacy of success. But..
      </p>
      <p class="hook3">
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

    <section class="section5">
      U.S. Best Performing Sports -- line graph
      <HomeAdv />
    </section>

    <section class="section6">
      <Athlete />
    </section>

    <section class="section7">
      <Country />
    </section>
    <!-- <section class = "section7">
      
      <Video />
    </section> -->
    <!-- <section>U.S. Gender Based Performance</section> -->
    <!-- <section class = "section8">Is there a home field advantage?</section> -->
    <section class="section9">
      Closing Thoughts

      <section id="custom-background-section">
        <h3 class="takeaway">
          The U.S. has been dominant in the Olympics thanks to excelling in
          <b>aquatics</b> and <b>athletics</b>, with the top Olympic athletes of
          all time earning their medals in those disciplines. However,
          <i> change is inevitable. </i>
          Decline in medals won can be seen in many of the U.S.'s top sports, and
          other countries are bound to catch up. However, improvement can be seen
          in <b> fencing, shooting, cycling, and taekwondo </b>. Alongside these
          rising sports are rising athletes, consistently helping drive success
          in these sports. Therefore, the U.S. should focus more on future
          investment and growth in
          <b> fencing, shooting, cycling, and taekwondo </b> in order to sustain
          the U.S.'s dominance in the Olympics.
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
      left: calc(
        100% + 100px
      ); /* End position (right edge of the screen plus logo width) */
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
    text-shadow:
      0px 0px 5px #30dce5,
      0px 0px 10px #30dce5,
      0px 0px 10px #30dce5,
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
    background-color: rgba(0, 0, 0, 0.7); /* Background color */
    padding: 10px 20px; /* Padding */
    display: inline-block; /* Display as inline-block */
    border: 2px solid #30dce5;
  }

  .hook2 {
    font-family: 'Times New Roman';
    line-height: 1.5;
    position: relative;
    top: -200px;
    color: white;
    background-color: rgba(0, 0, 0, 0.7); /* Background color */
    padding: 20px; /* Padding */
    border: 2px solid #30dce5; /* Border */
    box-shadow: 0 0 10px #30dce5; /* Box shadow */
    display: inline-block; /* Display as inline-block */
  }

  .hook3 {
    font-family: 'Times New Roman';
    line-height: 1.5;
    position: relative;
    top: -70px;
    color: white;
    background-color: rgba(0, 0, 0, 0.7); /* Background color */
    padding: 20px; /* Padding */
    border: 2px solid #30dce5; /* Border */
    box-shadow: 0 0 10px #30dce5; /* Box shadow */
    display: inline-block; /* Display as inline-block */
  }

  .foreground {
    width: 100%;
    margin: 0 auto;
    height: auto;
    position: relative;
    /* outline: blue 3px; */
  }
  section {
    height: 100vh;
    background-color: rgba(0, 0, 0, 0.1);
    color: white;
    /* outline: magenta solid 3px; */
    text-align: center;
    max-width: 100%; /* adjust at will */
    color: black;
    /* padding: 1em 1em; */
    /* margin: 2em 2em 2em 2em; */
    font-family: 'times new roman';
    text-align: center;
    font-size: 35px;
    line-height: 28px;
    font-weight: bold;
    padding-top: 10px;
  }
  .section1 {
    /* background-color: white; */
    font-size: 20px;
    background-image: url('b.jpeg');
    background-size: cover; /* Adjust to cover the entire section */
    background-position: center; /* Adjust to position the image */
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
    background-color: rgba(0, 0, 0, 0.7);
    padding: 5px 10px;
  }

  #custom-background-section {
    position: relative; /* Needed for the absolute positioning of the pseudo-element */
    height: 100%; /* Adjust based on your needs */
    overflow: hidden; /* Ensures the pseudo-element doesn't extend outside this container */
  }

  #custom-background-section::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-size: cover; /* Cover the entire section */
    background-position: center; /* Center the background image */
    opacity: 0.5; /* Adjust opacity as needed */
    z-index: -1; /* Ensures the background is behind the content */
    animation: switchBackground 15s infinite; /* Apply the animation */
  }

  /* #custom-background-section > * {
      position: relative;
      z-index: 1;
  } */

  @keyframes switchBackground {
    0%,
    20% {
      background-image: url('olympics4.jpeg');
    }
    45%,
    55% {
      background-image: url('olympics5.jpeg');
    }
    55%,
    70% {
      background-image: url('olympics6.jpeg');
    }
    100%,
    0% {
      background-image: url('olympics7.jpeg');
    }
  }
</style>
