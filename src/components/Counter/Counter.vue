<template>
  <div class="number-container d-flex justify-content-center align-items-center">
    <!-- 减 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="subtract" ref="subRef">-</button>
    <!-- 购买的数量 -->
    <span class="number-box">{{num}}</span>
    <!-- 加 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="add">+</button>
  </div>
</template>

<script>
  import bus from '@/components/eventBus.js'
export default {
  props:{
    num:{
      type:Number,
      default:0
    },
  },
  methods: {
    add(){
      this.$refs.subRef.disabled=false;
      this.$emit('count-change',this.num+1)
    },
    subtract(){
      if(this.num-1===0) {
        this.$refs.subRef.disabled=true;
      }
      this.$emit('count-change',this.num-1)
    }
  },
}
</script>

<style lang="less" scoped>
.number-box {
  min-width: 30px;
  text-align: center;
  margin: 0 5px;
  font-size: 12px;
}

.btn-sm {
  width: 30px;
}
</style>
