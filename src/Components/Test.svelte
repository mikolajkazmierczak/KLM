<script>
  let x = 0;
  let xPrev = 0;
  let y = 0;
  let yPrev = 0;

  let refresh = 200;

  let mouse = true;
  let enter = false;
  let focused = false;

  let startTime;
  let elapsed;

  let name = 'Mikołaj Dariusz Kaźmierczak';
  let text;
  let correct;

  setInterval(() => {
    if (x != xPrev || y != yPrev) {
      xPrev = x;
      yPrev = y;
      mouse = true;
      if (!running) start();
    } else {
      mouse = false;
    }
  }, refresh);

  let running = false;

  function handleMousemove(e) {
    x = e.clientX;
    y = e.clientY;
  }

  function handleKeydown(e) {
    if (e.key == 'Enter') {
      enter = true;
    }
  }
  function handleKeyup(e) {
    if (e.key == 'Enter') {
      stop();
      setTimeout(() => {
        enter = false;
      }, refresh);
    }
  }

  function handleFocus() {
    focused = true;
  }
  function handleBlur() {
    focused = false;
  }

  function start() {
    running = true;
    elapsed = null; // reset
    text = ''; // reset
    startTime = Date.now();
  }
  function stop() {
    running = false;
    const endTime = Date.now();
    elapsed = (endTime - startTime) / 1000;
    correct = text == name;
  }
</script>

<svelte:window
  on:mousemove={handleMousemove}
  on:keydown={handleKeydown}
  on:keyup={handleKeyup}
  on:focus={handleFocus}
  on:blur={handleBlur}
/>

<div class="bg" class:running />

<p>Pełne imię i nazwisko:<br /><input type="text" bind:value={name} /></p>

<br />
<p>
  {#if focused}
    Focused
  {:else}
    <span class="warning">Unfocused!</span>
  {/if}
</p>
<p>
  {#if running}
    <span class="active">Testing...</span>
  {:else}
    Idle
  {/if}
</p>

<br />
<p>x: {x}</p>
<p>y: {y}</p>
<p>mouse: <span class:active={mouse}>{mouse}</span></p>
<p>enter: <span class:active={enter}>{enter}</span></p>

<br />
<input class="target" type="text" bind:value={text} />

{#if elapsed}
  <br />
  <br />
  <p>{elapsed}</p>
  <p>correct: {correct}</p>
{/if}

<style>
  .bg {
    z-index: -1;
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
  }
  .running {
    background-color: rgb(230, 255, 230);
  }
  .active {
    color: rgb(33, 172, 33);
  }
  .warning {
    color: rgb(223, 68, 68);
  }
  .target {
    width: 40ch;
    padding: 0.5rem;
  }
</style>
