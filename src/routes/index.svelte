<script lang="ts">
  import Truths from '../data/test.json'

  type Truth = {
    id: string;
    sentence: string;
    is_true: string;
  }
  
  export let truths: Truth[] = Truths

  const GREEN = '#4CAF50'
  const LIGHT_GREEN = '#99ee99'
  const RED = '#FF5252'
  const LIGHT_RED = '#ee9999'

  let endGame = `<div class="animate__animated animate__zoomInDown">
                  game over <button id="replay">replay</button>
                </div>`

  let state = [
    0,
    0,
    0,
  ]

  const replay = () => {
    state = [
      0,
      0,
      0,
    ]
  }

  const choose = (truth: string, index: number) => {
    if (truth === '1') state[index] = 1
    else {
      state[index] = 2
      setTimeout(() => {
        const element = document.getElementById(`truth${index}`)
        if (element) element.outerHTML = endGame
        document.getElementById('replay')?.addEventListener('click', replay)
      }, 2000)
    }
  }

  const getClassname = (state: number) => {
    if (state === 1) return 'animate__animated animate__pulse'
    else if (state === 2) return 'animate__animated animate__hinge'
  }

  const getBackgroundColor = (state: number) => {
    if (state === 1) return LIGHT_GREEN
    else if (state === 2) return LIGHT_RED
  }

  const getBorderColor = (state: number) => {
    if (state === 1) return GREEN
    else if (state === 2) return RED
  }
  
</script>

<head>
  <link
    rel="stylesheet"
    href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css"
  />
</head>
<main>
  <div id="word">
    <h2 id="current-score">
      Click the two you think are true!
    </h2>
  </div>

  <div
    id="answer"
    style="min-height: 200px; display: flex; flex-direction: column; justify-content: space-evenly; margin: auto;"
  >
    {#each truths as truth, idx}
      <button
        id={`truth${idx}`}
        class={getClassname(state[idx])}
        on:click={() => choose(truth.is_true, idx)}
        style={`margin: 10px; border-color: ${getBorderColor(state[idx])}; background-color: ${getBackgroundColor(state[idx])};`}
      >{truth.sentence}</button>
    {/each}
  </div>
</main>

<style>

  :root {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen,
      Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  }

  #word {
    display: grid;
    line-height: 36px;
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: repeat(2, 1fr);
    margin: auto;
    width: fit-content;
  }

  #current-score {
    grid-row-start: 2;
    grid-column-start: 2;
    grid-column-end: 3;
  }

  #point-marker {
    grid-row-start: 2;
    grid-column-start: 3;
    grid-column-end: 4;
    text-align: left;
  }

  #info {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    border: #ff3e00 1px solid;
    max-width: 400px;
    padding: 20px;
  }

  .mastered {
    text-decoration: line-through 2px;
  }

  button {
    font-family: inherit;
    font-size: inherit;
    padding: 1em 2em;
    color: #ff3e00;
    background-color: rgba(255, 62, 0, 0.1);
    border-radius: 2em;
    border: 2px solid rgba(255, 62, 0, 0);
    outline: none;
    width: 200px;
    font-variant-numeric: tabular-nums;
    cursor: pointer;
  }

  main {
    text-align: center;
    padding: 1em;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
  }

  /* img {
    height: 16rem;
    width: 16rem;
  } */

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4rem;
    font-weight: 100;
    line-height: 1.1;
    margin: 2rem auto;
    max-width: 14rem;
  }

  @media (min-width: 480px) {
    h1 {
      max-width: none;
    }
  }
</style>
