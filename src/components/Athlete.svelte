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
{/if}

{#if showTop3}
  <div class="container-thompson">
    <button on:click={revealTop2} class="big-button">Reveal 2nd Place</button>
    <div class="thompson-info">
      <h4>Jenny Thompson</h4>
      <h2><u>Career Highlights</u></h2>
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
        <li>
          Set 6 World Records in long course meters
        </li>
      </ul>

      <!-- <h2>Records</h2>
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
      </ul> -->
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
    <h4>Birgit Fischer</h4>
    <h2><u>Career Highlights</u></h2>
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
    <h4>Michael Phelps</h4>
    <h2><u>Career Highlights</u></h2>
    <ul>
      <li>Most gold medals overall in World Championship history.</li>
      <li>
        Most gold medals won at a single Olympics: 8 gold medals in 2008 Beijing
        Olympics.
      </li>
      <li>Most Olympic medals of any athlete: 16 total medals.</li>
      <!-- <li>
        Most gold medals won in a single World Championship: 7 gold medals in
        2007.
      </li> -->
      
      <li>Currently holds the most world records in swimming with 7</li>
    </ul>

    <!-- <h2>Records</h2>
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
    </ul> -->
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
    /* border: 1px solid #ccc; */
    /* padding: 10px; */
    text-align: center;
    width: 100%; /* Set width for the container */
    height: 100vh;
    max-height: 100vh; /* Set a maximum height for the container */
    /* overflow: hidden;  */
    position: relative; /* Position the container relative to its parent */
    background-image: url('fischer.png');
    background-size: 48%;
    background-repeat: no-repeat;
    background-position: 0% 30%;
    background-color:
    linear-gradient(
      to bottom right,
      rgba(0, 0, 128, 0.5),
    rgba(0, 0, 0, 0.5)
    );
  }

  .container-thompson {
    /* border: 1px solid #ccc; */
    /* padding: 5px; */
    text-align: center;
    width: 100%; /* Set width for the container */
    height: 100vh;
    max-height: 100vh; /* Set a maximum height for the container */
    position: relative; /* Position the container relative to its parent */
    background-image: url('thompson.png');
    background-size: 30%;
    background-repeat: no-repeat; /* Prevent the background image from repeating */
    background-position: 5% 5%; /* Position the background image at the left bottom corner */
    background-color: 
    linear-gradient(
      to bottom right,
      rgba(0, 0, 128, 0.5),
    rgba(0, 0, 0, 0.5)
    );
  }

  .container-phelps {
    /* border: 1px solid #ccc; */
    /* padding: 10px; */
    text-align: center;
    width: 100%; /* Set width for the container */
    height: 100vh;
    max-height: 100vh; /* Set a maximum height for the container */
    overflow: hidden; /* Hide overflow content */
    position: relative; /* Position the container relative to its parent */
    background-image: url('phelps.png');
    background-size: 60%;
    background-repeat: no-repeat;
    background-position: -35% 0%;
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
    width: 450px; /* Set the width of the timeline */
    height: 86.5%; /* Set the height of the timeline to cover the entire container */
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
  /* .timeline::after {
    content: '';
    position: absolute;
    width: 6px;
    height: 100%; 
    background-color: white;
    top: 0;
    left: 50%;
    margin-left: -3px;
  } */

  /* Media queries - Responsive timeline on screens less than 600px wide */
  @media screen and (max-width: 600px) {
    /* Place the timelime to the left */
    .timeline::after {
      left: 31px;
    }
  }

  h2 {
    font-size: 45px;
  }
  
  h4 {
    font-size: 45px;
    font-weight: 500, 'bold';
    text-transform: uppercase;
    line-height: 1;
    text-align: center;
    font-family: 'times new roman';
    color: white;
  }

  ul {
    /* list-style-type: none; */
    padding: 0;
    margin: 0 auto;
    text-align: center;
    width: 35%; /* Set the width to the maximum content width */
    /* color: ; */
    font-size: 18px;
    list-style: none;
    margin-right: 30%;
  }

  * {
	box-sizing: border-box;
}

li + li {
	margin-top: .5rem;
}

li {
	display: flex;
	align-items: center;
	gap: 1rem;
	background: aliceblue;
	padding: 1rem;
	border-radius: 1rem;
	width: calc(100% - 5rem);
	box-shadow: 0.25rem 0.25rem 0.75rem rgb(0 0 0 / 0.1);
}

/* li::before {
	content: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 576 512' width='100' title='fish'%3E%3Cpath d='M327.1 96c-89.97 0-168.54 54.77-212.27 101.63L27.5 131.58c-12.13-9.18-30.24.6-27.14 14.66L24.54 256 .35 365.77c-3.1 14.06 15.01 23.83 27.14 14.66l87.33-66.05C158.55 361.23 237.13 416 327.1 416 464.56 416 576 288 576 256S464.56 96 327.1 96zm87.43 184c-13.25 0-24-10.75-24-24 0-13.26 10.75-24 24-24 13.26 0 24 10.74 24 24 0 13.25-10.75 24-24 24z' /%3E%3C/svg%3E");
} */

li:nth-child(even) {
	flex-direction: row-reverse;
	background: honeydew;
	/* margin-right: -2rem;
	margin-left: 2rem; */
}

li:nth-child(even)::before {
	transform: rotateY(180deg);
}
  /* ul ul {
    margin-left: 40px; 
    width: 100%;
    color: rgb(255, 255, 200);
  } */

</style>
