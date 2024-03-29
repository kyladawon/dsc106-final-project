<script>
  import Scroller from '@sveltejs/svelte-scroller';
  import Map from './Map.svelte';
  import { geoMercator } from 'd3-geo';
  import Project3 from './Proj3.svelte';
  import Athlete from './Athlete.svelte';
  import Top5 from './Top5.svelte';
  import { tweened } from 'svelte/motion';
  import { cubicOut } from 'svelte/easing';
  import { fade } from 'svelte/transition';
  import Bubble from './SportsBubble.svelte';
  import Country from './Country.svelte';
  import HomeAdv from './HomeAdv.svelte';
  import Rising from './Rising.svelte';

  import archery from './risingData/archery.js';
  import badminton from './risingData/badminton.js';
  import boxing from './risingData/boxing.js';
  import canoecayak from './risingData/canoecayak.js';
  import cycling from './risingData/cycling.js';
  import fencing from './risingData/fencing.js';
  import handball from './risingData/handball.js';
  import hockey from './risingData/hockey.js';
  import judo from './risingData/judo.js';
  import pentathlon from './risingData/pentathlon.js';
  import shooting from './risingData/shooting.js';
  import softball from './risingData/softball.js';
  import tabletennis from './risingData/tabletennis.js';
  import taekwondo from './risingData/taekwondo.js';
  import tennis from './risingData/tennis.js';
  import triathlon from './risingData/triathlon.js';
  import weightlifting from './risingData/weightlifting.js';
  import wrestling from './risingData/wrestling.js';
  import RisingAthlete from './RisingAthlete.svelte';

  let count, index, offset, progress;
  let width, height;
  let show = false;
  // let isMouseInside = false;

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

    <section class="section5">
      Home field advantage?<br />
      <HomeAdv />
    </section>

    <section class="section4">
      U.S. Best Performing Sports
      <Bubble />
    </section>

    <section class="section8">
      <h2>U.S. Rising Sports</h2>
      <label for="show" style="display: inline;">Show Line:</label>
      <input id="show" type="checkbox" bind:checked={show} />

      <div class="row">
        <Rising data={archery} {show} title="Archery" />
        <Rising data={badminton} {show} title="Badminton" />
        <Rising data={boxing} {show} title="Boxing" />
        <Rising data={canoecayak} {show} title="Canoecayak" />
      </div>
      <div class="row">
        <Rising data={cycling} {show} title="Cycling" />
        <Rising data={fencing} {show} title="Fencing" />
        <Rising data={handball} {show} title="Handball" />
        <Rising data={hockey} {show} title="Hockey" />
      </div>
      <div class="row">
        <Rising data={judo} {show} title="Judo" />
        <Rising data={pentathlon} {show} title="Pentathlon" />
        <Rising data={shooting} {show} title="Shooting" />
        <Rising data={softball} {show} title="Softball" />
      </div>
      <div class="row">
        <Rising data={tabletennis} {show} title="TableTennis" />
        <Rising data={taekwondo} {show} title="Taekwondo" />
        <Rising data={tennis} {show} title="Tennis" />
        <Rising data={triathlon} {show} title="Triathlon" />
      </div>
      <div class="row">
        <Rising data={weightlifting} {show} title="Weightlifting" />
        <Rising data={wrestling} {show} title="Wrestling" />
      </div>
    </section>

    <section>
      Top 5 Sports <br />
      <Top5 />
    </section>

    <section class="section6">
      <Athlete />
    </section>

    <section class="section7">
      Leading Country <br />
      <Country />
    </section>

    <section class="section10">
      Rising Athlete for Top 5 Rising Sport
      <RisingAthlete />
    </section>

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
  .section10 {
    background-image: linear-gradient(
      to bottom right,
      rgba(205, 12, 12, 0.5),
      rgba(28, 218, 91, 0.5)
    );
  }

  .section8 {
    width: 100%;
    overflow: auto; /* Add overflow to enable scrolling if content overflows */
    padding: 10px; /* Add padding to the section */
    background-image: linear-gradient(
      to bottom right,
      rgba(205, 12, 12, 0.5),
      rgba(28, 218, 91, 0.5)
    );
  }

  .row {
    display: flex;
    flex-wrap: wrap; /* Allow items to wrap onto multiple lines */
    margin-bottom: 30px; /* Reduce margin between rows */
    margin-left: 5cap;
    margin-top: 30px;
    /* justify-content: center;  */
    transition: justify-content 0.5s;
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
