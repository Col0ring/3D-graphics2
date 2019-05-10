<!-- button -->
<template>
  <div>
    <div id="btn">
      <ul>
        <li @click="Table">Table</li>
        <li @click="Sphere">Sphere</li>
        <li @click="Helix">Helix</li>
        <li @click="Grid">Grid</li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },
  methods: {
    Table() {
      this.$parent.id = 1;
    },
    Sphere() {
      this.$parent.id = 2;
    },
    Helix() {
      this.$parent.id = 3;
    },
    Grid() {
      this.$parent.id = 4;
    }
  }
};
</script>
<style lang='less' scoped>
/* btn按钮 */
#btn {
  position: absolute;
  bottom: 20px;
  left: 20px;
  width: 300px;
  height: 30px;
  /* border:2px solid #0f0; */
  color: #fff;

  ul li {
    float: left;
    margin: 0 5px;
    border: 1px solid rgba(127, 255, 255, 1);
    padding: 0 10px;
    line-height: 30px;
    border-radius: 5px;
    font-size: 12px;
    cursor: pointer;
    color: rgba(127, 255, 255, 1);
    &:hover {
      background-color: rgba(127, 255, 255, 0.5);
    }
  }
}
</style>