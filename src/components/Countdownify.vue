<template>
  <div id="wrap" :style="wrapStyle">
    <div>
      <div id="head">{{ msg }}<span class="normal">まで</span></div>
      <div id="strong-area">
        <span class="dgtl strong">{{ strong }}</span>
        <span class="dgtl unit">{{ unit }}</span>
      </div>
      <div id="detail-area" class="dgtl">{{ detail }}</div>
    </div>
  </div>
</template>

<script>
import "dseg/css/dseg.css";
import dayjs from "dayjs";
import duration from "dayjs/plugin/duration";
dayjs.extend(duration);
export default {
  name: "Countdownify",
  props: {
    msg: { type: String, default: "終了" },
    goalTime: { type: [String, Number, Array, Object], required: true },
    backgroundColor: { type: String, default: "black" },
  },
  data() {
    return {
      now: dayjs(),
      goal: dayjs(this.goalTime),
      timeoutID: -1,
    };
  },
  methods: {
    tick() {
      this.now = dayjs();
      const nextTick = this.goal.diff(this.now) <= 10000 ? 100 : 16; // 残り10秒切ったら細かくやる
      this.timeoutID = setTimeout(this.tick, nextTick);
    },
    untick() {
      clearTimeout(this.timeoutID);
    },
  },
  mounted: function () {
    this.tick();
  },
  computed: {
    diff: function () {
      return Math.max(this.goal.diff(this.now), 0);
    },
    detail: function () {
      const D = this.goal.diff(this.now, "d");
      return `${D}${dayjs
        .duration(this.diff)
        .format("[日] HH : mm : ss", { trim: false, trunc: true })}`;
    },
    strong: function () {
      let unit = "d";
      if (this.diff >= 1000 * 60 * 60 * 24) {
        // 1日以上なら日付を強調
        unit = "d";
      } else if (this.diff >= 1000 * 60 * 60) {
        // 1時間以上なら時間を強調
        unit = "h";
      } else if (this.diff >= 1000 * 60) {
        // 1分以上なら分を強調
        unit = "m";
      } else if (this.diff >= 1000 * 10) {
        // 10秒以上なら秒を強調
        unit = "s";
      } else if (this.diff >= 100) {
        // 10秒未満は小数点表示
        return (this.goal.diff(this.now) / 1000).toFixed(1);
      } else {
        // それ以外はゼロ表示にする
        return 0;
      }
      return `${this.goal.diff(this.now, unit)}`;
    },
    unit: function () {
      if (this.diff >= 1000 * 60 * 60 * 24) {
        // 1日以上なら日付を強調
        return ` 日`;
      } else if (this.diff >= 1000 * 60 * 60) {
        // 1時間以上なら時間を強調
        return ` 時間`;
      } else if (this.diff >= 1000 * 60) {
        // 1分以上なら分を強調
        return ` 分`;
      } else if (this.diff >= 0) {
        // 1秒以上なら秒を強調
        return ``;
      }
      // 異常値ならとりあえずなにもなし
      return ``;
    },
    wrapStyle: function () {
      return {
        backgroundColor: this.backgroundColor ? this.backgroundColor : "black",
      };
    },
  },
  watch: {
    now: function () {
      if (this.diff <= 0) {
        this.untick();
        this.$emit("its-time", this.now);
      }
    },
  },
};
</script>

<style scoped>
#wrap {
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}
#head {
  color: white;
  font-size: 1.5em;
}
#strong-area {
  margin: 3em;
}
.normal {
  font-size: 0.75em;
}
.strong {
  font-size: 10em;
  font-style: italic;
}
.unit {
  font-size: 3em;
}
#detail-area {
  font-size: 2em;
}
.dgtl {
  font-family: "DSEG14-Classic";
  font-weight: bold;
  color: #f2f804;
  text-shadow: 0px 0px 0.2em #f2f804;
}

@keyframes flash {
  0% {
    text-shadow: 0px 0px 0.2em #f2f804;
  }
  50% {
    text-shadow: 0px 0px 3em #f2f804;
  }
  100% {
    text-shadow: 0px 0px 0.2em #f2f804;
  }
}

.flashing {
  animation-name: flash;
  animation-timing-function: linear;
  animation-duration: 2s;
  animation-iteration-count: infinite;
}
</style>
