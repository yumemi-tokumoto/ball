<template>
  <div class="container">
    <h1>Nuxt</h1>
    <div>
      <select v-model="selectHand" name="hand">
        <option value="">未選択</option>
        <option>グー</option>
        <option>チョキ</option>
        <option>パー</option>
        <option>ほげ</option>
      </select>
      <p>選択中：{{ selectHand }}</p>
    </div>
    <button @click="setHand">手をセットする</button>
    <div v-if="hand" class="result">
      <p>{{ hand }}を出した！</p>
    </div>
    <div v-else class="result">
      <p>不正な値です！</p>
    </div>
    <div>
      <input v-model="test" type="text" />
      <p>{{ computedTest }}</p>
      <p>{{ testTest }}</p>
    </div>
  </div>
</template>

<script>
// import Logo from '~/components/Logo.vue'

export default {
  components: {},
  data() {
    return {
      selectHand: '',
      hand: false,
      test: 'aaaa',
      testTest: 'sss'
    }
  },
  computed: {
    computedTest: {
      set(newVal) {
        console.log(newVal + newVal)
        this.testTest = newVal + newVal
      },
      get() {
        return this.test + 'です'
      }
    }
  },
  methods: {
    setHand() {
      if (/^(グー|チョキ|パー)$/.test(this.hand)) {
        this.hand = this.selectHand
      } else {
        this.hand = false
      }
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
</style>
