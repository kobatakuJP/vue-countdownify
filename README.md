# vue-countdownify

設定した日時までカウントダウンするコンポーネントです。

## Install

### npm

```
$ npm install --save vue-countdownify
```

### import at javascript

```
import Countdownify from 'vue-countdownify';
import 'vue-countdownify/dist/vue-countdownify.css'
```

## Usage

### Props

|Name|Type|Required|Default|Description|
|:--|:--|:--|:--|:--|
|msg|String|false|"終了"|ヘッダに表示される文字列。「{{msg}}まで」と表示される。|
|goal-time|String|true|-|カウントダウンする先の時刻。[dayjs](https://day.js.org/en/)のコンストラクタに引数として渡すことができる形式で入力。<br>例：<br>`goal-time="2021-04-04T16:00:00.000Z"`<br>`:goal-time="[2021, 5, 4]`|

### Event

|Name|Description|
|:--|:--|
|its-time|設定時刻になるとイベントがEmitされます。引数にはEmit時点のdayjsオブジェクトが渡されます。例えばこのイベントを受け取ってイベントの画面へリダイレクトする等の使い方ができると思います。|

## Example

README上部のGifを実現するコードは以下（`App.vue`）。

```App.vue
<template>
  <div id="app">
    <Countdownify msg="イベント" :goalTime="Date.now() + 5000" @its-time="timeup"/>
  </div>
</template>

<script>
import Countdownify from 'vue-countdownify';
import 'vue-countdownify/dist/vue-countdownify.css'

export default {
  name: 'App',
  components: {
    Countdownify
  },
  methods: {
    timeup(time) {
      console.log("time is up!")
      alert(time)
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
```