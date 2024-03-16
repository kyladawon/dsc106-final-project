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
  import Top5 from './Top5.svelte';


  let count, index, offset, progress;
  let width, height;
  let show = false;

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

    

    <section class="intro-page">
      <!-- <div class="banner"> -->
        <h1 class="title">United States in the Olympics</h1>
        <p>Kyla Park and Essie Cheng</p>
      <!-- </div> -->
      <div class="box">
        <div class="inner">
          <span>Exploration Through Visualization</span>
        </div>
        <div class="inner">
          <span>Exploration Through Visualization</span>
        </div>
      </div>
      <h2 class="hook">
        <i>Nothing Lasts Forever </i>
      </h2>
      <div class="olympic-logo"></div>
    </section>

    <section class="proj3">
      <h3> Data Exploration</h3>
      <p class = 'instructions'> A comprehensive overview of the Olympics: click bar to drill down into further detail, click background to go back up. Select/deselect medal buttons to filter type of medal. </p>
      <Project3 />
      <p class = 'explanation'> No matter the sport or year, the U.S frequently appears. They seem to consistently be a dominant force, and maybe even be the best in the world. A look at the overall medal tally can confirm if this is true. ↓ </p>
    </section>

    <section class="home-advantage-1">
      <h3> Home Advantage </h3>
      <p class = 'instructions'> Medal count history for each country that has hosted the Olympics: use dropdown menu to select location. Red dot indicates that the country hosted the Olympics that year</p>
      <HomeAdv />
    </section>

    <section class="home-advantage-2">
      <h3> Home Advantage</h3>
      <p class = 'instructions'> A broader look at the connection between home advantage and overall Olympic success: 
      light blue countries have hosted the Olympics and are in the top ten for number of medals won overall. 
      Purple countries have hosted the Olympics but are not in the top ten for number of medals won overall.
      Orange countries have not hosted the Olympics but are in the top ten for number of medals won overall.
      Hover the markers for host city and year.
    </p>
      <Map bind:geoJsonToFit {index}/>
      <p class = 'explanation'> Might notice</p>
    </section>

    <section class="top-sports">
      <h3> Team USA Best Performing Sports</h3>
      <p class = 'instructions'> Top 5 sports for Team USA each year: scroll through slider to change between years or click button for automated transitions. </p>
      <Bubble />
      <p class = 'explanation'> Might notice</p>
    </section>

    <section class="top-athletes">
      <h3> Top Olympic Athletes</h3>
      <p class = 'instructions'> Top 3 Olympic athletes of all time: click the button to
      reveal each athlete. Then scroll the timeline on the right for more details about their feats. </p>
      <Athlete />
    </section>

    <section class="transition-section">
      <p > It looks like </p>
    </section>

    

    <section class="top-countries">
      <h3> Top Countries</h3>
      <p class = 'instructions'> This is snfjsn </p>
      <Country />
      <p class = 'explanation'> Might notice</p>
    </section>

    <section class="rising-sports">
      <h3>U.S. Rising Sports</h3>
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

    <section class = top-sports-rising>
      Top 5 Sports <br />
      <Top5 />
    </section>

   

    

    <!-- <section class="section10">
      Rising Athlete for Top 5 Rising Sport
      <RisingAthlete />
    </section> -->

    <!-- <section class = "section7">
      
      <Video />
    </section> -->
    <!-- <section>U.S. Gender Based Performance</section> -->
    <!-- <section class = "section8">Is there a home field advantage?</section> -->
    <section class="conclusion">
      

      
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
    
  </div>
</Scroller>

<style>

.box {
	display: flex;
}

.box .inner {
	width: 800px;
	height: 200px;
	line-height: 200px;
	font-size: 4em;
	font-family: sans-serif;
	font-weight: bold;
	white-space: nowrap;
	overflow: hidden;
}

.box .inner:first-child {
	background-color: lightskyblue;
	color: rgb(53, 127, 206);
	transform-origin: right;
	transform: perspective(100px) rotateY(-15deg);
}

.box .inner:last-child {
	background-color: lightskyblue;
	color: rgb(53, 127, 206);
	transform-origin: left;
	transform: perspective(100px) rotateY(15deg);
}

.box .inner span {
	position: absolute;
	animation: marquee 5s linear infinite;
}

.box .inner:first-child span {
	animation-delay: 2.5s;
	left: -100%;
}

@keyframes marquee {
	from {
		left: 100%;
	}

	to {
		left: -100%;
	}
}

  .olympic-logo {
    width: 120px; /* Adjust size as needed */
    height: 1200px; /* Adjust size as needed */
    background-image: url('logo.png'); /* Replace with the URL of your Olympic logo */
    background-size: contain;
    background-repeat: no-repeat;
    position: relative;
    bottom: -55px;
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
  /* .banner {
    background-color: rgba(0, 0, 0, 0.7); 
    padding: 20px; 
    margin-bottom: 20px; 
    text-align: center; 
  }
  .banner p {
    color: white;
  } */

  h3 {
    margin-top: 10px;
  }
  .instructions {
    font-size: 16px;
    margin-top: -20px;
    font-style: italic;
    font-family: arial;
    margin-left: 3px;
    margin-right: 3px;
  }
  .explanation {
    font-size: 18px;
    font-family: 'times new roman';
    margin-top: 30px;
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
    padding: 10px 20px; 
    display: inline-block; /* Display as inline-block */
    border: 2px solid #30dce5;
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
    padding-top: 20px;


  }
  .intro-page {
    /* background-color: white; */
    font-size: 20px;
    background-image: url('b.jpeg');
    background-size: cover; /* Adjust to cover the entire section */
    background-position: center; /* Adjust to position the image */
  }
  .proj3 {
    background-image: linear-gradient(
      to bottom right,
      rgba(0, 255, 0, 0.5),
      rgba(0, 100, 0, 0.5)
    );
  }

  .home-advantage-2 {
    background-image: linear-gradient(
      to bottom right,
      rgba(255, 255, 0, 0.5),
      rgba(255, 0, 0, 0.5)
    );
  }

  .top-sports {
    background-image: linear-gradient(
      to bottom right,
      rgba(0, 0, 255, 0.5),
      rgba(0, 255, 255, 0.5)
    );
  }
  .home-advantage-1 {
    background-image: linear-gradient(
      to bottom right,
      rgba(128, 0, 128, 0.5),
      rgba(255, 192, 203, 0.5)
    );
  }
  .top-athletes {
    background-image: linear-gradient(
      to bottom right,
      rgba(0, 0, 128, 0.5),
      rgba(0, 0, 0, 0.5)
    );
  }
  .transition-section {
    background-image: linear-gradient(
      to bottom right,
      rgba(127, 3, 104, 0.5),
      rgba(15, 201, 230, 0.5)
    );
  }
  .rising-sports {
    width: 100%;
    overflow: auto; 
    padding: 10px; 
    background-image: linear-gradient(
      to bottom right,
      rgba(205, 12, 12, 0.5),
      rgba(28, 218, 91, 0.5)
    );
  }

  .top-countries {
    background-image: linear-gradient(
      to bottom right,
      rgba(0, 0, 128, 0.5),
      rgba(0, 0, 0, 0.5)
    );
  }

  .row {
        font-size: 15px;
        display: flex;
        flex-wrap: wrap; /* Allow items to wrap onto multiple lines */
        justify-content: center; /* Center items horizontally */
        margin-bottom: 10px; /* Reduce margin between rows */
        margin-top: 30px;
    }



  .takeaway {
    color: white;
    line-height: 1.5;
    background-color: rgba(0, 0, 0, 0.7);
    padding: 5px 10px;
  }

  .conclusion {
    background-image: url('b.jpeg');
    background-size: cover; /* Adjust to cover the entire section */
    background-position: center; /* Adjust to position the image */

  }

  </style>


  