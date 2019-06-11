<template>
  <div class="container" @mousemove="mousemove">
    <div id="top-bar" class="bar top-bar" :style="{ left: x + 'px' }"></div>
    <div
      id="ball"
      class="ball"
      :style="{ left: ballX + 'px', top: ballY + 'px' }"
    ></div>
    <div
      id="bottom-bar"
      class="bar bottom-bar"
      :style="{ left: x + 'px' }"
    ></div>
    <div v-show="darkScreenIsVisible || failed" class="dark-screen">
      <p v-show="failed" class="failed">Failed!</p>
      <button class="start-button" @click="start">
        <span v-show="isPlayed">RE</span>START
      </button>
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
      isPlayed: false,
      darkScreenIsVisible: true,
      initialX: 0,
      x: '50%',
      barHalfWidth: 0,
      barHeight: 0,
      ballHalf: 0,
      ballX: '50%',
      ballY: '50%',
      pxByFrameDefault: 5,
      pxByFrame: 0,
      failed: false,
      add: {
        x: 0,
        y: 0
      }
    }
  },
  computed: {},
  methods: {
    start(e) {
      this.pxByFrame = this.pxByFrameDefault
      this.isStarted = true
      this.failed = false
      this.darkScreenIsVisible = false
      this.initialX = e.pageX
      const topBarElm = document.getElementById('top-bar')
      // const bottomBarElm = document.getElementById('bottom-bar')
      const ballElm = document.getElementById('ball')
      this.barHalfWidth = topBarElm.clientWidth / 2
      this.barHeight = topBarElm.clientHeight
      this.ballHalf = ballElm.clientWidth / 2
      const barHeightBallHalf = this.barHeight + this.ballHalf
      this.ballX = window.innerWidth / 2
      this.ballY = window.innerHeight / 2
      this.add.x = this.pxByFrame
      this.add.y = this.pxByFrame
      if (!this.isPlayed) {
        this.isPlayed = true
        setInterval(() => {
          if (!this.isStarted) return
          const windowInnerHeight = window.innerHeight
          const windowInnerHeightMinusBarHeightBallHalf =
            windowInnerHeight - barHeightBallHalf
          const windowInnerWidthMinusBallHalf =
            window.innerWidth - this.ballHalf
          const topBarRect = topBarElm.getBoundingClientRect()
          // const bottomBarRect = bottomBarElm.getBoundingClientRect()
          // const ballRect = ballRect.getBoundingClientRect()
          const inBarWidth =
            topBarRect.left < this.ballX && this.ballX < topBarRect.right

          if (this.ballX < this.ballHalf) {
            if (this.add.x !== this.pxByFrame) {
              this.add.x = this.pxByFrame
              this.addXChange()
            }
          }
          if (windowInnerWidthMinusBallHalf < this.ballX) {
            if (this.add.x !== this.pxByFrame * -1) {
              this.add.x = this.pxByFrame * -1
              this.addXChange()
            }
          }
          if (inBarWidth) {
            if (this.ballY < barHeightBallHalf) {
              this.add.y = this.pxByFrame
            }
            if (windowInnerHeightMinusBarHeightBallHalf < this.ballY) {
              this.add.y = this.pxByFrame * -1
            }
          } else if (this.ballY < 0 || windowInnerHeight < this.ballY) {
            this.failed = true
            this.isStarted = false
          }
          this.ballX += this.add.x
          this.ballY += this.add.y
        }, 20)
      }
    },
    addXChange() {
      this.pxByFrame += 1
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
  background-image: linear-gradient(#cff, #fff);
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
.failed {
  font-size: 40px;
  font-weight: bold;
  color: red;
  position: absolute;
  top: 20%;
  text-align: center;
  left: 0;
  width: 100%;
  z-index: 20;
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
