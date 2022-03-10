<template>
  <div v-if="props.results" class="results-container">
    <div class="result-container user">
      <p>You Picked</p>
      <div :class="['results-user', { winner: userWins }]">
        <div
          :class="['option', props.results.userPlay.name, { show: showUser }]"
        >
          <img :src="props.results.userPlay.imgUrl" alt="" />
        </div>
      </div>
    </div>
    <div :class="['results-info', { show: showInfo }]">
      <p class="winner-text">{{ winnerText }}</p>
      <button class="play-again-btn" @click="$emit('play-again')">
        Play Again
      </button>
    </div>
    <div class="result-container comp">
      <p>the house Picked</p>
      <div :class="['results-comp', { winner: compWins }]">
        <div
          :class="['option', props.results.compuPlay.name, { show: showComp }]"
        >
          <img :src="props.results.compuPlay.imgUrl" alt="" />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
  import { computed, inject, ref, onMounted } from 'vue'
  const props = defineProps(['results'])
  const playOptions = inject('play-options')

  const showUser = ref(false)
  const showComp = ref(false)
  const userWins = ref(false)
  const compWins = ref(false)
  const showInfo = ref(false)

  const userPlay = playOptions.filter(
    (play) => play.name === props.results.userPlay.name
  )[0]

  const winnerText = computed(() => {
    if (props.results.winner === 'comp') {
      return 'You lose'
    } else if (props.results.winner === 'user') {
      return 'You win'
    } else {
      return 'Draw'
    }
  })
  const score = inject('score')
  const updateScore = inject('update-score')

  onMounted(() => {
    setTimeout(() => {
      showUser.value = true
    }, 100)
    setTimeout(() => {
      showComp.value = true
    }, 2000)
    setTimeout(() => {
      userWins.value = props.results.winner === 'user'
      compWins.value = props.results.winner === 'comp'
      showInfo.value = true
      if (props.results.winner !== 'draw') {
        if (userWins.value) {
          score.value++
        }
        if (compWins.value) {
          score.value--
        }
        updateScore()
      }
    }, 2200)
  })
</script>

<style lang="scss" scoped>
  .results-container {
    flex: 1 1 auto;
    height: 100%;
    width: 100%;
    display: flex;
    justify-content: space-between;
    // align-items: center;
    flex-wrap: wrap;

    @media (min-width: 376px) {
      width: 100%;
      max-width: 945px; //var(--desktop-width);
      align-items: center;
    }
  }

  .results-info {
    order: 3;
    flex: 1 1 100%;
    width: 33%;
    overflow: hidden;
    transform: scaleX(0);
    transition: 100ms ease-in;
    &.show {
      width: 100%;
      transform: scaleX(1);
    }
    @media (min-width: 376px) {
      order: 2;
      max-width: 33%;
    }
  }

  .winner-text {
    text-transform: uppercase;
    font-size: 5.6rem;
    color: #fff;
    line-height: 1;
    margin: 0 0 2rem;
    text-align: center;
  }

  .play-again-btn {
    width: 20rem;
    height: 4.8rem;
    display: flex;
    justify-content: center;
    align-items: center;
    text-transform: uppercase;
    font-family: inherit;
    font-size: 1.7rem;
    letter-spacing: 1.1px;
    text-align: center;
    margin: 0 auto;
    border: none;
    border-radius: 1rem;
  }

  .option {
    $min-border: 12px;
    $rate-border: 3.5vw;
    $max-border: 20px;
    @media (min-width: 376px) {
      $min-border: 12px;
      $rate-border: 3.5vw;
      $max-border: 30px;
    }
    width: clamp(100px, 25vw, 300px);
    border: solid clamp($min-border, $rate-border, $max-border) transparent;
    box-shadow: 0px 10px 0 0 rgba($color: #000000, $alpha: 0.2) inset;
    transform: scale(0);
    &.show {
      transform: scale(1);
    }
    img {
      width: 50%;
    }

    &::after {
      box-shadow: 0px 10px 0 0 rgba($color: #000000, $alpha: 0.2) inset;
    }

    &::before {
      margin: calc(
        -1 * clamp($min-border, $rate-border, $max-border)
      ); /* !importanté */
      border-radius: inherit; /* !importanté */
      box-shadow: 0px calc((0.5vw + 2px) * -1) 0 0
        rgba($color: #000000, $alpha: 0.4) inset;
    }
  }
  .result-container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    order: 1;

    p {
      order: 2;
      text-align: center;
      margin-bottom: 2rem;
      text-transform: uppercase;
      font-size: 2rem;
      color: #fff;
      letter-spacing: 1.1px;
      @media (min-width: 376px) {
        order: 1;
      }
    }
    &.user {
      order: 1;
    }
    &.comp {
      order: 2;
      @media (min-width: 376px) {
        order: 3;
      }
    }
  }
  .results-user {
    order: 1;
    transition: all 100ms ease-in-out;
    @media (min-width: 376px) {
      order: 2;
    }
  }
  .results-comp {
    order: 1;
    transition: all 100ms ease-in-out;
    background: rgba($color: #000000, $alpha: 0.2);
    border-radius: 50%;

    @media (min-width: 376px) {
      order: 2;
    }
  }

  .winner {
    position: relative;
    border-radius: 50%;
    box-shadow: 0 0 0 20px rgba($color: #fff, $alpha: 0.03),
      0 0 0 50px rgba($color: #fff, $alpha: 0.03),
      0 0 0 80px rgba($color: #fff, $alpha: 0.03);
    @media (min-width: 376px) {
      box-shadow: 0 0 0 50px rgba($color: #fff, $alpha: 0.03),
        0 0 0 120px rgba($color: #fff, $alpha: 0.03),
        0 0 0 190px rgba($color: #fff, $alpha: 0.03);
    }
  }
</style>
