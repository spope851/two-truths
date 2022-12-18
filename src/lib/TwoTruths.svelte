<script lang="ts">
  type Truth = {
    id: number;
    sentence: string;
    is_true: string;
  }

  let state = [
    0,
    0,
    0,
  ]

  let correctAnswers = 0
  
  let truths: Truth[] = [{
    "id":0,
    "sentence":"...loading",
    "is_true":"1"
  }]

  export let getTruths: () => Promise<Truth[]>

  const replay = async () => {
    truths = await getTruths() || truths
    state = [
      0,
      0,
      0,
    ]
    correctAnswers = 0
  }

  replay()

  const GREEN = '#4CAF50'
  const LIGHT_GREEN = '#99ee99'
  const RED = '#FF5252'
  const LIGHT_RED = '#ee9999'

  const getBackgroundColor = (state: number) => {
    if (state === 1) return LIGHT_GREEN
    else if (state === 2) return LIGHT_RED
  }

  const getBorderColor = (state: number) => {
    if (state === 1) return GREEN
    else if (state === 2) return RED
  }

  const choose = (truth: string, index: number) => {
    const element = document.getElementById(`truth${index}`)
    if (truth === '1') {
      correctAnswers ++
      state[index] = 1
      if (element) {
        element.classList.remove('animate__fadeIn')
        element.classList.add('animate__pulse')
      }
    }
    else {
      if (element) {
        element.classList.add('animate__hinge')
        element.setAttribute('style', `border-color: ${getBorderColor(2)}; background-color: ${getBackgroundColor(2)};`)
      }
      setTimeout(() => {
        state[index] = 2
      }, 2000)
    }
  }

  </script>

  <head>
  <link
    rel="stylesheet"
    href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css"
  />
  </head>
  <main>
  <div id="game" class="animate__animated">
    <h2>
      Guess which two are true!
    </h2>
    {#each truths as truth, idx}
      {#if state[idx] < 2}
        <button
          id={`truth${idx}`}
          class="animate__animated animate__fadeIn"
          on:click={() => choose(truth.is_true, idx)}
          style={`margin: 10px; border-color: ${getBorderColor(state[idx])}; background-color: ${getBackgroundColor(state[idx])};`}
          disabled={(correctAnswers === 2) || (state.includes(2))}
        >{truth.sentence}</button>
      {:else}
        <div class="animate__animated animate__zoomInDown" style="display: flex; align-items: center;">
          game over! <button style="padding: 10px; margin: 10px;" id="replay" on:click={replay}><svg width="24px" fill="#ff3e00" clip-rule="evenodd" fill-rule="evenodd" stroke-linejoin="round" stroke-miterlimit="2" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="m3.508 6.726c1.765-2.836 4.911-4.726 8.495-4.726 5.518 0 9.997 4.48 9.997 9.997 0 5.519-4.479 9.999-9.997 9.999-5.245 0-9.553-4.048-9.966-9.188-.024-.302.189-.811.749-.811.391 0 .715.3.747.69.351 4.369 4.012 7.809 8.47 7.809 4.69 0 8.497-3.808 8.497-8.499 0-4.689-3.807-8.497-8.497-8.497-3.037 0-5.704 1.597-7.206 3.995l1.991.005c.414 0 .75.336.75.75s-.336.75-.75.75h-4.033c-.414 0-.75-.336-.75-.75v-4.049c0-.414.336-.75.75-.75s.75.335.75.75z" fill-rule="nonzero"/></svg></button>
        </div>
      {/if}
    {/each}
    {#if correctAnswers === 2}
      <div class="animate__animated animate__zoomInDown" style="display: flex; align-items: center;">
        you win! <button style="padding: 10px; margin: 10px;" id="replay" on:click={replay}><svg width="24px" fill="#ff3e00" clip-rule="evenodd" fill-rule="evenodd" stroke-linejoin="round" stroke-miterlimit="2" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="m3.508 6.726c1.765-2.836 4.911-4.726 8.495-4.726 5.518 0 9.997 4.48 9.997 9.997 0 5.519-4.479 9.999-9.997 9.999-5.245 0-9.553-4.048-9.966-9.188-.024-.302.189-.811.749-.811.391 0 .715.3.747.69.351 4.369 4.012 7.809 8.47 7.809 4.69 0 8.497-3.808 8.497-8.499 0-4.689-3.807-8.497-8.497-8.497-3.037 0-5.704 1.597-7.206 3.995l1.991.005c.414 0 .75.336.75.75s-.336.75-.75.75h-4.033c-.414 0-.75-.336-.75-.75v-4.049c0-.414.336-.75.75-.75s.75.335.75.75z" fill-rule="nonzero"/></svg></button>
      </div>
    {/if}
  </div>
  </main>

  <style>
    :root {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen,
        Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
    }

    #game {
      display: grid;
      grid-auto-rows: 1fr;
      margin: auto;
      place-items: center;
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
  </style>
