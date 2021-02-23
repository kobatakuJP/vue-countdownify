<template>
  <div id="app">
    <Countdownify msg="フェスティバル開始" :goalTime="'2021-2-23 15:45:00'" @its-time="a"/>
  </div>
</template>

<script>
import Countdownify from './components/Countdownify.vue'

export default {
  name: 'App',
  components: {
    Countdownify
  },
  methods: {
    a(a) {
      alert(a)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
