<template>
  <section class="container">
    <div class="wrap">
      <div class="game-container">
        <div class="header">
          <div class="game-name">
            15
          </div>
          <div class="progress">
            <div class="title">
              Ходов
            </div>
            <div class="counter">
              {{ moves }}
            </div>
          </div>
          <div class="time">
            <div class="title">
              Время
            </div>
            <div class="counter">
              {{ 10 > mins ? '0' + mins : mins }}:{{ 10 > secs ? '0' + secs : secs }}
            </div>
          </div>
        </div>
        <game @on-move='moves++' @new-game='newGame' />
      </div>
    </div>
  </section>
</template>

<script>
import Game from './game.vue'

export default {
  components: {
    Game
  },
  data() {
    return {
      moves: 0,
      minutes: 0,
      seconds: 0
    }
  },
  computed: {
    mins() {
      return Math.floor(this.seconds / 60)
    },
    secs() {
      return this.seconds % 60
    }
  },
  created() {
    this.time()
  },
  methods: {
    time() {
      const x = this
      setInterval(function() {
        x.seconds++
      }, 1000)
    },
    newGame() {
      this.moves = 0
      this.seconds = 0
      this.minutes = 0
    }
  },
}
</script>

<style lang='scss'>
@import '@/assets/styles/idx.scss';
</style>
