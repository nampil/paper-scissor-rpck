<template>
  <div class="main-container">
    <div class="container">
      <header class="top-board">
        <img
          src="./assets/images/logo-bonus.svg"
          alt="Rock Scissors Paper Lizard Spock"
          class="logo"
        />
        <div class="score-box">
          <p class="score-title">Score</p>
          <p class="score-value">{{ score }}</p>
        </div>
      </header>
      <div class="zone-wrapper">
        <transition>
          <play-zone v-if="!showResults" @user-plays="handlePlay"></play-zone>
          <results v-else @play-again="handlePlayAgain" :results="results" />
        </transition>
      </div>
      <footer>
        <button class="rules-btn" @click.stop="showRules = !showRules">
          Rules
        </button>

        <modal v-if="showRules" @close="showRules = false" />
      </footer>
    </div>
  </div>
</template>

<script setup>
  import PlayZone from './components/PlayZone.vue'
  import { ref, computed, provide, onMounted } from 'vue'
  import Modal from './components/Modal.vue'
  import Results from './components/Results.vue'
  import scissorsImg from './assets/images/icon-scissors.svg'
  import spockImg from './assets/images/icon-spock.svg'
  import paperImg from './assets/images/icon-paper.svg'
  import lizardImg from './assets/images/icon-lizard.svg'
  import rockImg from './assets/images/icon-rock.svg'

  const showResults = ref(false)
  const showRules = ref(false)

  const playOptions = ref([
    {
      name: 'scissors',
      wins: ['paper', 'lizard'],
      imgUrl: scissorsImg,
    },
    {
      name: 'spock',
      wins: ['scissors', 'rock'],
      imgUrl: spockImg,
    },
    {
      name: 'paper',
      wins: ['rock', 'spock'],
      imgUrl: paperImg,
    },
    {
      name: 'lizard',
      wins: ['spock', 'paper'],
      imgUrl: lizardImg,
    },
    {
      name: 'rock',
      wins: ['lizard', 'scissors'],
      imgUrl: rockImg,
    },
  ])
  provide('play-options', playOptions.value)
  const compuPlay = ref('')
  const userPlay = ref('')
  const winner = ref('')
  const score = ref(0)
  provide('score', score)

  const results = computed(() => {
    return {
      userPlay: userPlay.value,
      compuPlay: compuPlay.value,
      winner: winner.value,
    }
  })

  const handlePlayAgain = () => {
    showResults.value = false
  }

  const handlePlay = (option) => {
    userPlay.value = playOptions.value.filter((play) => play.name === option)[0]
    calcComputerPlay()
  }

  const calcComputerPlay = () => {
    const index = Math.floor(
      Math.random(0, playOptions.value.length + 1) * playOptions.value.length
    )
    compuPlay.value = playOptions.value[index]
    getResult()
  }
  const getResult = () => {
    const user = userPlay.value
    const comp = compuPlay.value
    winner.value = 'draw'

    switch (user.name) {
      case 'scissors':
        if (comp.name !== 'scissors') {
          if (comp.wins.includes('scissors')) {
            winner.value = 'comp'
          } else {
            winner.value = 'user'
          }
        }
        break
      case 'paper':
        if (comp.name !== 'paper') {
          if (comp.wins.includes('paper')) {
            winner.value = 'comp'
          } else {
            winner.value = 'user'
          }
        }
        break
      case 'rock':
        if (comp.name !== 'rock') {
          if (comp.wins.includes('rock')) {
            winner.value = 'comp'
          } else {
            winner.value = 'user'
          }
        }
        break
      case 'lizard':
        if (comp.name !== 'lizard') {
          if (comp.wins.includes('lizard')) {
            winner.value = 'comp'
          } else {
            winner.value = 'user'
          }
        }
        break
      case 'spock':
        if (comp.name !== 'spock') {
          if (comp.wins.includes('spock')) {
            winner.value = 'comp'
          } else {
            winner.value = 'user'
          }
        }
        break
    }
    finalize()
  }
  const finalize = () => {
    showResults.value = true
  }

  const getScore = () => {
    const localData = JSON.parse(localStorage.getItem('score'))
    if (!localData) {
      updateScore()
    } else {
      score.value = parseInt(localData)
    }

    // eslint-disable-next-line
    console.log('localData: ', localData)
  }

  provide('get-score', getScore)
  const updateScore = () => {
    localStorage.setItem('score', score.value)
  }
  provide('update-score', updateScore)

  onMounted(() => {
    getScore()
  })
</script>
<style lang="scss">
  *,
  *::after,
  *::before {
    box-sizing: border-box;
  }
  :root {
    /* Neutral */
    --dark-text: hsl(229, 25%, 31%);
    --score-text: hsl(229, 64%, 46%);
    --header-outline: hsl(217, 16%, 45%);
    /* Background */
    --bg-rad-grad: hsl(214, 47%, 23%), hsl(237, 49%, 15%);

    /* Primary */
    --scissors-gradient: hsl(40, 84%, 53%), hsl(39, 89%, 49%);
    --paper-gradient: hsl(230, 89%, 65%), hsl(230, 89%, 62%);
    --rock-gradient: hsl(349, 70%, 56%), hsl(349, 71%, 52%);
    --lizard-gradient: hsl(261, 72%, 63%), hsl(261, 73%, 60%);
    --cyan: hsl(189, 58%, 57%), hsl(189, 59%, 53%);

    /* Media Width */
    --desktop-width: 1266px;
    --mobile-width: 375px;

    /* Fonts */
    --font-family: 'Barlow Semi Condensed', sans-serif;
    --font-weight-base: 600;
    --font-weight-bold: 700;
  }
  html {
    font-size: 10px;
  }
  body {
    margin: 0;
    font-family: var(--font-family);
    font-weight: var(--font-weight-base);
    background-image: radial-gradient(circle at 50% 10%, var(--bg-rad-grad));
    /* background-image: url('./assets/images/mobile-step-1-bonus.jpg'); */
    background-repeat: no-repeat;
    color: var(--dark-text);
  }
  .main-container {
    height: 100vh;
    overflow: hidden;
  }
  .container {
    max-width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    // justify-content: center;
    align-items: center;
    margin: 0 auto;
    padding: 3rem;
  }
  .top-board {
    display: flex;
    justify-content: space-between;
    align-items: center;
    border: 3px solid hsl(217, 16%, 45%);
    border-radius: 7px;
    padding: clamp(1rem, 4vw, 2rem);
    flex: 0 0 auto;
    @media (min-width: 375px) {
      width: 100%;
      max-width: 706px; //var(--desktop-width);
    }
  }
  .logo {
    /* display: none; */
    padding: 1.2rem;
    width: clamp(7.5rem, 20vw, 11.5rem);
  }
  .score-box {
    background-color: #fff;
    text-align: center;
    width: clamp(80px, 20vw, 150px);
    aspect-ratio: 1.3;
    border-radius: 0.5rem;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }
  .score-box p {
    margin: 0;
    line-height: 1;
  }
  .score-title {
    text-transform: uppercase;
    font-size: clamp(1rem, 4vw, 1.7rem);
    letter-spacing: 1px;
  }
  .score-value {
    font-size: clamp(4rem, 10vw, 6.4rem);
  }
  .zone-wrapper {
    flex: 1 1 auto;
    position: relative;
    max-height: 700px;
    @media (min-width: 375px) {
      width: 100%;
      max-width: 945px; //var(--desktop-width);
    }
  }

  .play-zone {
    display: grid;
    place-items: center;
    height: 100%;
  }
  .play-content {
    background-image: url('./assets/images/bg-pentagon.svg');
    background-position: 50% 50%;
    background-repeat: no-repeat;
    background-size: 75%;
    position: relative;
    z-index: 1;
    width: 100%;
    max-width: 480px;
    aspect-ratio: 1;
    max-height: 100%;
    display: flex;
    flex-direction: column;
  }
  .first-row {
    flex: 1 1 auto;
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .second-row {
    flex: 1 1 auto;
    width: 100%;
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: -2.5%;
    margin-bottom: 7.5%;
  }
  .third-row {
    flex: 1 1 auto;
    width: 100%;
    display: flex;
    justify-content: space-around;
    align-items: center;
    padding-inline: 3rem;
  }
  .option {
    $min-border: 12px;
    $rate-border: 3.5vw;
    $max-border: 20px;
    border: solid clamp($min-border, $rate-border, $max-border) transparent;
    background-clip: padding-box;
    padding: 1rem;
    border-radius: 50%;
    width: clamp(100px, 25vw, 150px);
    aspect-ratio: 1;
    position: relative;
    box-sizing: border-box;
    background-color: #fff;
    box-shadow: 0px 5px 0 0 rgba($color: #000000, $alpha: 0.2) inset;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 200ms ease-in-out;

    img {
      max-width: calc(100% - 2rem);
      height: auto;
      display: block;
    }
    &::after {
      content: '';
      position: absolute;
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;
      background-color: #fff;
      border-radius: inherit; /* !importanté */
      z-index: -1;
      box-shadow: 0px 5px 0 0 rgba($color: #000000, $alpha: 0.2) inset;
    }
    &::before {
      content: '';
      position: absolute;
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;
      z-index: -1;
      margin: calc(
        -1 * clamp($min-border, $rate-border, $max-border)
      ); /* !importanté */
      border-radius: inherit; /* !importanté */
      // background-image: linear-gradient(to right, red, orange);
      box-shadow: 0px -6px 0 0 rgba($color: #000000, $alpha: 0.4) inset;
    }
    &.btn {
      cursor: pointer;
      &:hover {
        transform: scale(1.05);
      }
    }
  }
  .scissors {
    &::before {
      background-image: linear-gradient(var(--scissors-gradient));
    }
  }
  .spock {
    &::before {
      background-image: linear-gradient(var(--cyan));
    }
  }
  .paper {
    &::before {
      background-image: linear-gradient(var(--paper-gradient));
    }
  }
  .lizard {
    &::before {
      background-image: linear-gradient(var(--lizard-gradient));
    }
  }
  .rock {
    &::before {
      background-image: linear-gradient(var(--rock-gradient));
    }
  }
  footer {
    flex: 0 0 auto;
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center;
    padding-bottom: 2.55rem;
    @media (min-width: 376px) {
      position: absolute;
      bottom: 40px;
      right: 40px;
      padding-bottom: 0;
    }
  }
  .rules-btn {
    text-align: center;
    background-color: transparent;
    border: 2px solid var(--header-outline);
    padding: 1rem;
    color: #fff;
    font-family: inherit;
    font-size: 1.5rem;
    text-transform: uppercase;
    letter-spacing: 3px;
    border-radius: 0.9rem;
    width: 130px;
    cursor: pointer;
  }

  // Animation

  .v-enter-active,
  .v-leave-active {
    transition: opacity 100ms ease;
    position: absolute;
    inset: 0;
  }

  .v-enter-from,
  .v-leave-to {
    opacity: 0;
  }
</style>
