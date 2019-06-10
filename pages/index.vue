<template>
  <div class="container">
    <h1>Nuxt</h1>
    <div>
      <select v-model="selectHand" name="selectHand">
        <option>グー</option>
        <option>チョキ</option>
        <option>パー</option>
        <option>ほげ</option>
      </select>
      <button @click="setHand">値をセット</button>
    </div>
    <div v-if="hand" class="result">
      <p>{{ hand }}を出した！</p>
    </div>
    <div v-else class="result">
      <p>不正な値です！</p>
    </div>
  </div>
</template>

<script>
// import Logo from '~/components/Logo.vue'

export default {
  components: {},
  data() {
    return {
      selectHand: 'グー',
      hand: 'グー'
    }
  },
  computed: {
    computedHand: {
      get() {
        return this.hand
      },
      set(newVal) {
        if (/^(グー|チョキ|パー)$/.test(newVal)) {
          this.hand = newVal
          console.log(this.hand)
        } else {
          this.hand = false
        }
      }
    }
  },
  methods: {
    setHand() {
      this.computedHand = this.selectHand
    }
  }
}
</script>

<style lang="scss" scoped>
* {
  font-size: 16px;
  padding: 10px;
  margin: 10px;
}
.container {
  h1 {
    font-weight: normal;
  }
}
.error {
  color: red;
}
</style>
