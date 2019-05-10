<!-- list组件 -->
<template>
  <!-- 从根组件获取到值 -->
  <ul
    class="list"
    :style="'transform:translateZ('+this.$parent.trZ+'px) rotateX('+this.$parent.rX+'deg) rotateY('+this.$parent.rY+'deg);'"
  >
    <!-- this.$parent.$parent.trZ等都是App中改变后传输的 -->
    <li v-for="(item,index) in 125" :key="index" :style="doStyle(index)">{{index}}</li>
  </ul>
</template>

<script>
export default {
  data() {
    return {
      firstEnter: true,
      arr1: [1, 3, 7, 9, 11, 14, 21, 16, 12, 10, 9, 7, 4, 1], //圆的所有数加起来为125,Sphere用
      arr2: [
        { x: 0, y: 0 },
        { x: 17, y: 0 },
        { x: 0, y: 1 },
        { x: 1, y: 1 },
        { x: 12, y: 1 },
        { x: 13, y: 1 },
        { x: 14, y: 1 },
        { x: 15, y: 1 },
        { x: 16, y: 1 },
        { x: 17, y: 1 },
        { x: 0, y: 2 },
        { x: 1, y: 2 },
        { x: 12, y: 2 },
        { x: 13, y: 2 },
        { x: 14, y: 2 },
        { x: 15, y: 2 },
        { x: 16, y: 2 },
        { x: 17, y: 2 }
      ]
    };
  },
  methods: {
    doStyle(index) {
      //用App的属性来记录全局的样式,注意用this.$parent.$parent是可以直接修改父级数据的
      if (this.firstEnter) {
        return this.doRandomStyle();
      } else {
        switch (this.$parent.id) {
          case 1:
            return this.doTable(index);
            break;
          case 2:
            return this.doSphere(index);
          case 3:
            return this.doHelix(index);
            break;
          case 4:
            return this.doGrid(index);
            break;
        }
      }
    },
    doRandom() {
      return Math.random() * 6000 - 2000;
    },
    doRandomStyle() {
      //刚开始进入的随机位置
      return (
        "transform:translate3D(" +
        this.doRandom() +
        "px," +
        this.doRandom() +
        "px," +
        this.doRandom() +
        "px)"
      );
    },
    doGrid(index) {
      return (
        "transform:translate3D(" +
        this.GridX(index) +
        "px," +
        this.GridY(index) +
        "px," +
        this.GridZ(index) +
        "px)"
      );
    },
    GridX(index) {
      return ((index % 5) - 2) * 200;
    },
    GridY(index) {
      return (Math.floor((index % 25) / 5) - 2) * 200;
    },
    GridZ(index) {
      return (Math.floor(index / 25) - 2) * 800;
    },
    doHelix(index) {
      //125为总个数
      let num = 125 / 4; //4行
      let deg = 360 / num;
      let mid = 125 / 2;
      let tY = 7;
      return (
        "transform:rotateY(" +
        index * deg +
        "deg) translateY(" +
        (index - mid) * tY +
        "px) translateZ(800px)"
      );
    },
    doSphere(index) {
      //numC处于哪一层   numCol处于哪一个列
      let numRow = 0,
        numCol = 0,
        arrSum = 0;
      for (let j = 0; j < this.arr1.length; j++) {
        //循环数组,判断已排序好的arr个数
        arrSum += this.arr1[j]; //已排序的层数的总个数
        if (arrSum > index) {
          //如果遍历到的排序层数总个数大于了当前的索引个数
          numRow = j; //当前索引就在现在遍历的层数上
          numCol = this.arr1[j] - (arrSum - index); //当前索引在当前层的列数
          //arrSum-index为当前遍历层数总个数与当前索引的差值,代表了离该层最后一个列的个数
          //this.arr[j]为当前列的总个数,减去前面的值就是当前的li在该层所在的列数
          break;
        }
      }
      let yDeg = 360 / this.arr1[numRow]; //当前层每一列应该旋转的平均度数
      let xDeg = 180 / (this.arr1.length - 1); //每一层平均占的位置度数大小
      return (
        //numCol-1.3改变每一层旋转的幅度,增加斜率
        "transform:rotateY(" +
        (numCol - 1.3) * yDeg +
        "deg) rotateX(" +
        (90 - numRow * xDeg) +
        "deg) translateZ(800px)"
        //90-numRow*xDeg能够丛上到下分布li,numRow越大在越下面,而且值为负数,最后一个为-90度,因为如果不用90来减,第一个li就在正中间,而不能丛上到下分布li
      );
    },
    doTable(index) {
      return (
        "transform:translate3D(" +
        this.TableX(index) +
        "px," +
        this.TableY(index) +
        "px,0px)"
      );
    },
    TableX(index) {
      //元素周期表
      let x;
      let midX = 18 / 2; //一行有18个，中间的值就是9
      if (index < 18) {
        x = this.arr2[index].x;
      } else {
        x = index % 18;
      }
      return (x - midX) * 180;
    },
    TableY(index) {
      let y;
      let midY = Math.ceil(125 / 18) / 2;
      if (index < 18) {
        y = this.arr2[index].y;
      } else {
        y = Math.floor(index / 18) + 2;
      }
      return (y - midY) * 210;
    }
  },
  mounted() {
    //页面挂载后的定时器转换
    setTimeout(() => {
      this.firstEnter = false;
    }, 500);
  }
};
</script>
<style lang='less' scoped>
ul {
  position: absolute;
  top: 50%;
  left: 50%;
  margin-left: -60px;
  margin-top: -80px;
  width: 120px;
  height: 160px;
  transform: rotateY(0deg);
  transform-style: preserve-3D;
  transform: translateZ(-1800px) rotateX(0deg) rotateY(0deg);

  li {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 127, 127, 0.5);
    border: 1px solid rgba(127, 255, 255, 0.25);
    box-shadow: 0 0 12px rgba(0, 255, 255, 0.5);
    line-height: 160px;
    text-align: center;
    font-size: 20px;
    font-weight: bold;
    color: #fff;
    transition: all 2s cubic-bezier(0.94, 0.02, 0.49, 0.98);
  }
}
</style>