<template>
  <div class="container" @mousemove="mousemove">
    <div class="bar top-bar" :style="{ left: x + 'px' }"></div>
    <div class="ball" :style="{ left: ballX + 'px', top: ballY + 'px' }"></div>
    <div class="bar bottom-bar" :style="{ left: x + 'px' }"></div>
    <div v-show="darkScreenIsVisible || failed" class="dark-screen">
      <button class="start-button" @click="start">START</button>
    </div>
  </div>
</template>

<script>
// import Logo from '~/components/Logo.vue'

export default {
  components: {},
  data() {
    return {
      isStarted: false,
      darkScreenIsVisible: true,
      initialX: 0,
      x: '50%',
      barHalfWidth: 0,
      barHeight: 0,
      ballHalf: 0,
      ballX: '50%',
      ballY: '50%',
      pxByFrame: 3,
      add: {
        x: 0,
        y: 0
      }
    }
  },
  computed: {},
  methods: {
    start(e) {
      this.isStarted = true
      this.darkScreenIsVisible = false
      this.initialX = e.pageX
      const barElm = document.querySelector('.bar')
      const ballElm = document.querySelector('.ball')
      this.barHalfWidth = barElm.clientWidth / 2
      this.barHeight = barElm.clientHeight
      this.ballHalf = ballElm.clientWidth / 2
      const barHeightBallHalf = this.barHeight + this.ballHalf
      this.ballX = window.innerWidth / 2
      this.ballY = window.innerHeight / 2
      this.add.x = this.pxByFrame
      this.add.y = this.pxByFrame
      setInterval(() => {
        const windowInnerHeightMinusBarHeightBallHalf =
          window.innerHeight - barHeightBallHalf
        const windowInnerWidthMinusBallHalf = window.innerWidth - this.ballHalf
        const barRect = barElm.getBoundingClientRect()
        // const ballRect = ballRect.getBoundingClientRect()
        const inBarXY = barRect.right < this.ballX || this.ballX < barRect.right
        if (inBarXY) {
          if (this.ballX < this.ballHalf) {
            this.add.x = this.pxByFrame
          }
          if (windowInnerWidthMinusBallHalf < this.ballX) {
            this.add.x = this.pxByFrame * -1
          }
          if (this.ballY < barHeightBallHalf) {
            this.add.y = this.pxByFrame
          }
          if (windowInnerHeightMinusBarHeightBallHalf < this.ballY) {
            this.add.y = this.pxByFrame * -1
          }
        }
        this.ballX += this.add.x
        this.ballY += this.add.y

        // if (true) {
        //   this.failed = true
        // }
      }, 20)
    },
    mousemove(e) {
      if (!this.isStarted) return
      const windowInnerWidthMinusbarHalfWidthWidth =
        window.innerWidth - this.barHalfWidth
      if (e.pageX < this.barHalfWidth) {
        this.x = this.barHalfWidth
      } else if (windowInnerWidthMinusbarHalfWidthWidth < e.pageX) {
        this.x = windowInnerWidthMinusbarHalfWidthWidth
      } else {
        this.x = e.pageX
      }
    },
    mounted() {
      this.add.x = this.pxByFrame
      this.add.y = this.pxByFrame
    }
  }
}
</script>

<style lang="scss" scoped>
.container {
  position: relative;
  height: 100vh;
}
.ball {
  position: absolute;
  border-radius: 200px;
  width: 30px;
  height: 30px;
  transform: translate(-50%, -50%);
  left: 50%;
  top: 50%;
  background-color: #000000;
}
.bar {
  position: absolute;
  height: 20px;
  width: 100px;
  background-color: #333;
  left: 50%;
  transform: translate(-50%);
}
.top-bar {
  top: 0;
}
.bottom-bar {
  bottom: 0;
}

.dark-screen {
  position: fixed;
  z-index: 10;
  background-color: rgba(0, 0, 0, 0.7);
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
}

.start-button {
  font-size: 30px;
  font-weight: bold;
  padding: 0.4em;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 20;
  cursor: pointer;
}
</style>
