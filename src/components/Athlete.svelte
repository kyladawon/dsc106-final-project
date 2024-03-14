<script>
  import { onDestroy } from 'svelte';
  import { tweened } from 'svelte/motion';
  import * as easingFns from 'svelte/easing';
  import Events from './Events.svelte';
  import { phelpsEvent } from './phelps.js';
  import { fischerEvent } from './fischer.js';
  import { thompsonEvent } from './thompson.js';

  // Define reactive variables to control visibility

  let showButton = true;
  let showTop3 = false;
  let showTop2 = false;
  let showTop1 = false;

  // Function to reveal Top 3 container
  function revealTop3() {
    showButton = false; // Hide the button
    showTop3 = true; // Show the Top 3 container
    showTop2 = false;
    showTop1 = false;
  }

  // Function to reveal Top 2 container
  function revealTop2() {
    showTop3 = false; // Hide the Top 3 container
    showTop2 = true; // Show the Top 2 container
    showTop1 = false;
  }

  function revealTop1() {
    showTop3 = false;
    showTop2 = false; // Hide the Top 3 container
    showTop1 = true; // Show the Top 2 container
  }

  let showInfo = false;
  let info = '';
  let currentMode = 'medal';

  function toggleMode(mode) {
    currentMode = mode;
  }

  const names = Object.keys(easingFns).filter(
    (n) => !['default', '__moduleExports'].includes(n)
  );

  let easingName = 'circInOut';
  let positionStore;
  let position = 0;
  let cleanup = null;

  const thompsonInfo = '';
  const phelpsInfo = '';
  const fischerInfo = '';

</script>




{#if showInfo}
  <div class="medal-info">
    {info}
  </div>
{/if}

{#if showButton}
  <button on:click={revealTop3} class="big-button">Reveal 3rd Place</button>
  <h2>Click button to reveal top 3 Olympic athletes</h2>
  <br />
  <h2>Scroll the timeline on the right for more information</h2>
{/if}

{#if showTop3}
  <div class="container-thompson">
    <button on:click={revealTop2} class="big-button">Reveal 2nd Place</button>

    <h3>Top 3</h3>
    <div class="thompson-info">
      <h4>Jenny Thompson</h4>
      <h2>Career Highlights</h2>
      <ul>
        <li>
          Competed in four consecutive Olympic Games, earning a total of 12
          medals, including eight golds.
        </li>
        <li>Named the World Swimmer of the Year in 1998.</li>
        <li>
          Competed collegiately at Stanford, earning 19 NCAA individual and
          relay titles.
        </li>
      </ul>

      <h2>Records</h2>
      <ul>
        <li>
          Set 6 World Records in long course meters:
          <ul>
            <li>100m freestyle</li>
            <li>100m butterfly</li>
            <li>4x100m freestyle (twice)</li>
            <li>4x100m medley relay (twice)</li>
          </ul>
        </li>
        <li>
          9 World Records in short course meters:
          <ul>
            <li>50m butterfly (three times)</li>
            <li>100m butterfly (four times)</li>
            <li>100m individual medley (twice)</li>
          </ul>
        </li>
      </ul>
    </div>
    <div class="timeline">
      {#each thompsonEvent as { Year, City, Event, Medal }}
        <Events year={Year} city={City} event={Event} medal={Medal} />
      {/each}
    </div>
  </div>
{/if}

{#if showTop2}
  <div class="container-fischer">
    <button on:click={revealTop3} class="big-button"
      >Go Back to 3rd Place</button
    >
    <button on:click={revealTop1} class="big-button">Reveal 1st Place</button>
    <h3>Top 2</h3>
    <h4>Birgit Fischer</h4>
    <h2>Career Highlights</h2>
    <ul>
      <li>
        Most Olympic Medals among Olympic kayakers: 12 Olympic medals (6 gold, 4
        silver, 2 bronze)
      </li>
      <li>Only Woman to win Olympic medals 20 years apart.</li>
      <li>
        Most Consecutive World Championship Wins: Fischer won three consecutive
        World Championships from 1981 to 1983.
      </li>
    </ul>
    <div class="timeline">
      {#each fischerEvent as { Year, City, Event, Medal }}
        <Events year={Year} city={City} event={Event} medal={Medal} />
      {/each}
    </div>
  </div>
{/if}

{#if showTop1}
  <div class="container-phelps">
    <button on:click={revealTop2} class="big-button"
      >Go Back to 2nd Place</button
    >
    <h3>Top 1</h3>
    <h4>Michael Phelps</h4>
    <h2>Career Highlights</h2>
    <ul>
      <li>
        Most gold medals won at a single Olympics: 8 gold medals in 2008 Beijing
        Olympics.
      </li>
      <li>Most Olympic medals of any athlete: 16 total medals.</li>
      <li>
        Most gold medals won in a single World Championship: 7 gold medals in
        2007.
      </li>
      <li>Most gold medals overall in World Championship history.</li>
    </ul>

    <h2>Records</h2>
    <ul>
      <li>
        The most decorated Olympian of all time with a total of 16 Olympic
        medals.
      </li>
      <li>
        Currently holds the most world records in swimming with seven:
        <ul>
          <li>100m butterfly LC</li>
          <li>200m butterfly LC</li>
          <li>400m individual medley LC</li>
          <li>4×100m freestyle relay LC</li>
          <li>4×200m freestyle relay LC</li>
          <li>4×100m medley relay LC</li>
          <li>4×100m freestyle relay SC</li>
        </ul>
      </li>
    </ul>
    <div class="timeline">
      {#each phelpsEvent as { Year, City, Event, Medal }}
        <Events year={Year} city={City} event={Event} medal={Medal} />
      {/each}
    </div>
  </div>
{/if}

<style>
  /* Style the button */
  .big-button {
    padding: 0.6em 2em;
    border: none;
    outline: none;
    color: rgb(255, 255, 255);
    background: #111;
    cursor: pointer;
    position: relative;
    z-index: 0;
    border-radius: 10px;
    user-select: none;
    -webkit-user-select: none;
    touch-action: manipulation;
  }
  .big-button:before {
    content: '';
    background: linear-gradient(
      45deg,
      #ff0000,
      #ff7300,
      #fffb00,
      #48ff00,
      #00ffd5,
      #002bff,
      #7a00ff,
      #ff00c8,
      #ff0000
    );
    position: absolute;
    top: -2px;
    left: -2px;
    background-size: 400%;
    z-index: -1;
    filter: blur(5px);
    -webkit-filter: blur(5px);
    width: calc(100% + 4px);
    height: calc(100% + 4px);
    animation: glowing-big-button 20s linear infinite;
    transition: opacity 0.3s ease-in-out;
    border-radius: 10px;
  }
  @keyframes glowing-big-button {
    0% {
      background-position: 0 0;
    }
    50% {
      background-position: 400% 0;
    }
    100% {
      background-position: 0 0;
    }
  }
  .big-button:after {
    z-index: -1;
    content: '';
    position: absolute;
    width: 100%;
    height: 100%;
    background: #222;
    left: 0;
    top: 0;
    border-radius: 10px;
  }

  .container-fischer {
    border: 1px solid #ccc;
    /* padding: 10px; */
    text-align: center;
    width: 100%; /* Set width for the container */
    height: 90vh;
    max-height: 90vh; /* Set a maximum height for the container */
    /* overflow: hidden;  */
    position: relative; /* Position the container relative to its parent */
    background-image: url('fischer.png');
    background-size: 48%;
    background-repeat: no-repeat;
    background-position: 0% 400%;
    background-color:
    linear-gradient(
      to bottom right,
      rgba(0, 0, 128, 0.5),
    rgba(0, 0, 0, 0.5)
    );
  }

  .container-thompson {
    border: 1px solid #ccc;
    /* padding: 5px; */
    text-align: center;
    width: 100%; /* Set width for the container */
    height: 90vh;
    max-height: 90vh; /* Set a maximum height for the container */
    position: relative; /* Position the container relative to its parent */
    background-image: url('thompson.png');
    background-size: 30%;
    background-repeat: no-repeat; /* Prevent the background image from repeating */
    background-position: 5% 100%; /* Position the background image at the left bottom corner */
    background-color: 
    linear-gradient(
      to bottom right,
      rgba(0, 0, 128, 0.5),
    rgba(0, 0, 0, 0.5)
    );
  }

  .container-phelps {
    border: 1px solid #ccc;
    /* padding: 10px; */
    text-align: center;
    width: 100%; /* Set width for the container */
    height: 90vh;
    max-height: 90vh; /* Set a maximum height for the container */
    overflow: hidden; /* Hide overflow content */
    position: relative; /* Position the container relative to its parent */
    background-image: url('phelps.png');
    background-size: 60%;
    background-repeat: no-repeat;
    background-position: -35% 100%;
    background-color:
    linear-gradient(
      to bottom right,
      rgba(0, 0, 128, 0.5),
    rgba(0, 0, 0, 0.5)
    );
  }

  .timeline {
    position: absolute;
    top: 0; /* Align the timeline to the top */
    right: 0; /* Align the timeline to the right */
    width: 500px; /* Set the width of the timeline */
    height: 100%; /* Set the height of the timeline to cover the entire container */
    background-color: 
    linear-gradient(
      to bottom right,
      rgba(0, 0, 128, 0.5),
    rgba(0, 0, 0, 0.5)
    );
    font-family: Helvetica, sans-serif;
    overflow-x: hidden; /* Hide horizontal scrollbar */
    overflow-y: auto; /* Enable vertical scrolling */
    font-size: 14px;
    font-family: times new roman;
  }

  /* The actual timeline (the vertical ruler) */
  .timeline::after {
    content: '';
    position: absolute;
    width: 6px;
    height: 100%; /* Set the height of the timeline to cover the entire container */
    background-color: white;
    top: 0;
    left: 50%;
    margin-left: -3px;
  }

  /* Media queries - Responsive timeline on screens less than 600px wide */
  @media screen and (max-width: 600px) {
    /* Place the timelime to the left */
    .timeline::after {
      left: 31px;
    }
  }

  h4 {
    font-size: 43px;
    font-weight: 500, 'bold';
    text-transform: uppercase;
    line-height: 1;
    text-align: center;
    font-family: 'verdana';
    color: white;
  }

  ul {
    /* list-style-type: none; */
    padding: 0;
    margin: 0 auto;
    text-align: left;
    width: 35%; /* Set the width to the maximum content width */
    color: rgb(255, 255, 200);
    font-size: 15px;
  }

  ul ul {
    margin-left: 40px; /* Adjust the left margin of the nested ul */
    width: 100%;
    color: rgb(255, 255, 200);
  }
</style>
