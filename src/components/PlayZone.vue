<template>
  <main class="play-zone">
    <div class="play-content">
      <div class="first-row">
        <button class="option scissors btn" @click="handlePlay('scissors')">
          <img :src="scissors.imgUrl" alt="" />
        </button>
      </div>
      <div class="second-row">
        <button class="option spock btn" @click="handlePlay('spock')">
          <img :src="spock.imgUrl" alt="" />
        </button>
        <button class="option paper btn" @click="handlePlay('paper')">
          <img :src="paper.imgUrl" alt="" />
        </button>
      </div>
      <div class="third-row">
        <button class="option lizard btn" @click="handlePlay('lizard')">
          <img :src="lizard.imgUrl" alt="" />
        </button>
        <button class="option rock btn" @click="handlePlay('rock')">
          <img :src="rock.imgUrl" alt="" />
        </button>
      </div>
    </div>
  </main>
</template>

<script setup>
  import { inject } from 'vue'

  const emit = defineEmits(['user-plays'])
  const playOptions = inject('play-options')
  const scissors = playOptions.filter((play) => play.name === 'scissors')[0]
  const spock = playOptions.filter((play) => play.name === 'spock')[0]
  const paper = playOptions.filter((play) => play.name === 'paper')[0]
  const lizard = playOptions.filter((play) => play.name === 'lizard')[0]
  const rock = playOptions.filter((play) => play.name === 'rock')[0]

  const handlePlay = (play) => {
    setTimeout(() => {
      emit('user-plays', play)
    }, 0)
  }
</script>

<style lang="scss" scoped></style>
