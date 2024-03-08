<script>
  import { onMount } from 'svelte';
  // DOM Elements
  let slider;

  // State
  let isDragging = false,
    startPos = 0,
    currentTranslate = 0,
    prevTranslate = 0,
    animationID = 0,
    currentIndex = 0;

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

  onMount(() => {
    const slides = document.querySelectorAll('div');
    slides.forEach((slide, index) => {
      // Touch event listeners
      slide.addEventListener('touchstart', touchStart(index));
      slide.addEventListener('touchend', touchEnd);
      slide.addEventListener('touchmove', touchMove);
      // Mouse event listeners
      slide.addEventListener('mousedown', touchStart(index));
      slide.addEventListener('mouseup', touchEnd);
      slide.addEventListener('mouseleave', touchEnd);
      slide.addEventListener('mousemove', touchMove);
    });

    // Event callbacks
    function touchStart(index) {
      return function (event) {
        currentIndex = index;
        startPos = getPositionX(event);
        isDragging = true;
        animationID = requestAnimationFrame(animation);
        slider.classList.add('grabbing');
      };
    }

    function touchEnd() {
      isDragging = false;
      cancelAnimationFrame(animationID);
      const movedBy = currentTranslate - prevTranslate;
      if (movedBy < -100 && currentIndex < slides.length - 1) currentIndex += 1;
      if (movedBy > 100 && currentIndex > 0) currentIndex -= 1;
      setPositionByIndex();
      slider.classList.remove('grabbing');
    }

    function touchMove(event) {
      if (isDragging) {
        const currentPosition = getPositionX(event);
        currentTranslate = prevTranslate + currentPosition - startPos;
      }
    }

    // Helper functions
    function getPositionX(event) {
      return event.type.includes('mouse')
        ? event.pageX
        : event.touches[0].clientX;
    }

    function animation() {
      setSliderPosition();
      isDragging && requestAnimationFrame(animation);
    }

    function setSliderPosition() {
      slider.style.transform = `translateX(${currentTranslate}px)`;
    }

    function setPositionByIndex() {
      currentTranslate = currentIndex * -window.innerWidth;
      prevTranslate = currentTranslate;
      setSliderPosition();
    }
    function prevSlide() {
      if (currentIndex > 0) {
        currentIndex--;
        setPositionByIndex();
      }
    }

    function nextSlide() {
      if (currentIndex < items.length - 1) {
        currentIndex++;
        setPositionByIndex();
      }
    }
  });
</script>

<svelte:window on:contextmenu|preventDefault|stopPropagation />

<article bind:this={slider}>
  {#each items as item, i}
    {#if item.type === 'video'}
      <div class="slide">
        <iframe
          src={`https://www.youtube.com/embed/${item.videoId}`}
          width="560"
          height="315"
          frameborder="0"
          allowfullscreen
        ></iframe>
        <button on:click={prevSlide}>Previous</button>
        <button on:click={nextSlide}>Next</button>
      </div>
    {/if}
  {/each}
</article>

<style>
  article {
    display: inline-flex;
    overflow: hidden;
    transform: translateX(0);
    transition: transform 0.3s ease-out;
    cursor: grab;
  }

  .slide {
    flex: 0 0 auto;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    width: 100%; /* Occupy full width of parent container */
  }

  iframe {
    width: 100%; /* Occupy full width of parent container */
    height: 100%; /* Occupy full height of parent container */
    max-width: 100%;
    max-height: 100%;
  }

  div {
    max-height: 100vh;
    width: 100vw;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 1rem;
    user-select: none;
  }

  img {
    max-width: 100%;
    max-height: 50%;
    transition: transform 0.3s ease-in-out;
  }

  h2 {
    font-size: 2.5rem;
    margin-bottom: 0rem;
  }

  h4 {
    font-size: 1.3rem;
  }

  button {
    background-color: #444;
    color: #fff;
    text-decoration: none;
    padding: 1rem 1.5rem;
    margin-top: 1rem;
    cursor: pointer;
  }

  :global(.grabbing img) {
    transform: scale(0.9);
  }
</style>
