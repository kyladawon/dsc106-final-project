<!-- <script>
  import { onMount } from 'svelte';
  import { afterUpdate } from 'svelte';

  let items = [
    {
      type: 'video',
      videoId: 'llDWiCHGBAQ', // Example video ID, replace with your own
    },
    {
      type: 'video',
      videoId: 'UHE2e9jyZh4', // Example video ID, replace with your own
    },
    {
      type: 'video',
      videoId: 'OeGVCdY35JY', // Example video ID, replace with your own
    },
  ];

  let index = 0;
  let autoSlide = true;
  let interval;

  function nextSlide() {
    index = (index + 1) % items.length;
  }

  function prevSlide() {
    index = (index - 1 + items.length) % items.length;
  }

  function handleDotClick(dotIndex) {
    index = dotIndex;
  }

  function startAutoSlide() {
    interval = setInterval(nextSlide, 4000);
  }

  function stopAutoSlide() {
    clearInterval(interval);
  }

  onMount(startAutoSlide);

  afterUpdate(() => {
    if (autoSlide) {
      startAutoSlide();
    } else {
      stopAutoSlide();
    }
  });
</script>

<div
  class="slider"
  on:mouseenter={stopAutoSlide}
  on:mouseleave={startAutoSlide}
>
  {#each items as item, i}
    {#if i === index}
      {#if item.type === 'video'}
        <div class="slide">
          <iframe
            src={`https://www.youtube.com/embed/${item.videoId}`}
            width="560"
            height="315"
            frameborder="0"
            allowfullscreen
          ></iframe>
        </div>
      {/if}
    {/if}
  {/each}
  <div class="dots">
    {#each items as _, i}
      <span
        class="dot"
        class:selected={index === i}
        on:click={() => handleDotClick(i)}
      ></span>
    {/each}
  </div>
  <button class="prev" on:click={prevSlide}>Previous</button>
  <button class="next" on:click={nextSlide}>Next</button>
</div>

<style>
  .slider {
    position: relative;
    width: 100%;
    height: 50vh;
    overflow: hidden;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .slide {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }

  .dots {
    position: absolute;
    bottom: 20px;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
  }

  .dot {
    width: 10px;
    height: 10px;
    background-color: #ccc;
    border-radius: 50%;
    margin: 0 5px;
    cursor: pointer;
  }

  .dot.selected {
    background-color: #333;
  }

  .prev,
  .next {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    background: none;
    border: none;
    cursor: pointer;
    font-size: 16px;
  }

  .prev {
    left: 20px;
  }

  .next {
    right: 20px;
  }
</style> -->

<!-- boxing, pentathlon, shooting, handball, fencing -->
fencing: ZAGUNIS, Mariel
