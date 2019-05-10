<template>
  <div
    id="app"
    @drag.prevent
    @selectstart.prevent
    @mousewheel="doScroll"
    @mousedown="doMouseDown"
    @mousemove="MouseMove"
    @mouseup="doMouseUp"
  >
    <v-List></v-List>
    <v-Button></v-Button>
  </div>
</template>

<script>
import Button from "./components/button.vue";
import List from "./components/list.vue";

export default {
  name: "App",
  data() {
    return {
      trZ: -1800, //与滚动视图有关
      rX: 0, //与拖动视图有关
      rY: 0,
      roX: 0,
      roY: 0,
      startX: 0,
      startY: 0,
      doMove: false, //判断鼠标是否在移动
      id: 1
    };
  },
  components: {
    "v-Button": Button,
    "v-List": List
  },
  computed: {
    MouseMove() {
      // 如果不能移动就赋值一个空函数
      return this.doMove ? this.doMouseMove : () => {};
    }
  },
  methods: {
    doChange() {},
    doMouseDown(e) {
      this.doMove = true;
      this.startX = e.clientX;
      this.startY = e.clientY;
    },
    doMouseMove(e) {
      let chanX = e.clientX - this.startX;
      let chanY = e.clientY - this.startY;
      this.rX = this.roX - chanY * 0.2;
      this.rY = this.roY - chanX * 0.2;
    },
    doMouseUp(e) {
      this.roX = this.rX;
      this.roY = this.rY;
      this.doMove = false;
    },
    doScroll(e) {
      this.trZ += (e.wheelDelta / 120) * 100;
      this.trZ = Math.min(2200, this.trZ);
      this.trZ = Math.max(-8000, this.trZ);
    }
  },
  mounted() {
    // 兼容火狐
    this.$el.addEventListener(
      "DOMMouseScroll",
      e => {
        this.trZ -= (e.detail / 3) * 100;
        this.trZ = Math.min(2200, this.trZ);
        this.trZ = Math.max(-8000, this.trZ);
      },
      false
    );
  }
};
</script>

<style lang='less' scoped>
#app {
  width: 100%;
  height: 100%;
  background: #023b3b url("../static/images/bg.png") no-repeat center
    center/cover;
  perspective: 800px; /* 景深： */
}
</style>
