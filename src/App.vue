<template>
  <div id="app">
    <Countdownify msg="フェスティバル開始" :goalTime="Date.now() + 10000" @its-time="a"/>
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
html, body {
  height: 100%;
  width: 100%;
  margin: 0;
  padding: 0;
}
#app {
  height: 100%;
}
</style>
