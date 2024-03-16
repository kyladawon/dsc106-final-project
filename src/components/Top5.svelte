<script>
    let boxing = false;
    let fencing = false;
    let handball = false;
    let pentathlon = false;
    let shooting = false;
  
    const handleMouseenter = () => (
      (boxing = true),
      (fencing = true),
      (handball = true),
      (pentathlon = true),
      (shooting = true)
    );
    const handleMouseleave = () => (
      (boxing = false),
      (fencing = false),
      (handball = false),
      (pentathlon = false),
      (shooting = false)
    );
  </script>
  
  <div style="display: flex; flex-wrap: wrap;">
    <div
      style="margin-right: 10px;"
      on:mouseenter={() => handleMouseenter('boxing')}
      on:mouseleave={handleMouseleave}
    >
      <img
        class:curious={boxing}
        alt="boxing"
        src="boxing.png"
        style="width: 20%; height: auto;"
      />
      <p style="visibility: {boxing ? 'visible' : 'hidden'};">Boxing</p>
    </div>
    <div
      style="margin-right: 10px;"
      on:mouseenter={() => handleMouseenter('fencing')}
      on:mouseleave={handleMouseleave}
    >
      <img
        class:curious={fencing}
        alt="fencing"
        src="fencing.png"
        style="width: 20%; height: auto;"
      />
      <p style="visibility: {fencing ? 'visible' : 'hidden'};">Fencing</p>
    </div>
    <div
      style="margin-right: 10px;"
      on:mouseenter={() => handleMouseenter('handball')}
      on:mouseleave={handleMouseleave}
    >
      <img
        class:curious={handball}
        alt="handball"
        src="handball.png"
        style="width: 20%; height: auto;"
      />
      <p style="visibility: {handball ? 'visible' : 'hidden'};">Handball</p>
    </div>
  </div>
  <div style="display: flex; flex-wrap: wrap;">
    <div
      style="margin-right: 10px;"
      on:mouseenter={() => handleMouseenter('pentathlon')}
      on:mouseleave={handleMouseleave}
    >
      <img
        class:curious={pentathlon}
        alt="pentathlon"
        src="pentathlon.png"
        style="width: 20%; height: auto;"
      />
      <p style="visibility: {pentathlon ? 'visible' : 'hidden'};">Pentathlon</p>
    </div>
    <div
      style="margin-right: 10px;"
      on:mouseenter={() => handleMouseenter('shooting')}
      on:mouseleave={handleMouseleave}
    >
      <img
        class:curious={shooting}
        alt="shooting"
        src="shooting.png"
        style="width: 20%; height: auto;"
      />
      <p style="visibility: {shooting ? 'visible' : 'hidden'};">Shooting</p>
    </div>
  </div>
  
  <style>
    img {
      position: relative;
      left: 0;
      bottom: -60px;
      transform: translate(-320%, 0) rotate(-40deg);
      transform-origin: 100% 100%;
      transition: transform 0.4s;
    }
  
    .curious {
      transform: translate(0%, 0) rotate(0deg);
    }
  </style>