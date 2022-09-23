<template>
  <div class="app-container">
    <!-- 实例化header组件 -->
    <Header></Header>
    <!-- 实例化Goods组件 -->
    <Goods
     v-for="item in list" 
     :key="item.id" 
     :id="item.goods_id"
     :title="item.goods_name" 
     :pic="item.goods_img" 
     :price="item.goods_price"
     :state="item.goods_state"
     :count="item.goods_count"
     @state-change="getNewState"
     >
      <Counter :num="item.goods_count" @count-change="getNewCount(item,$event)"></Counter>
    </Goods>

     <!-- 实例化Footer组件 -->
     <Footer :checkState="fullChecked"  @state-change="getFooterStateChange" :amount="awt" :allCount="allCount"></Footer>

  </div>
</template>

<script>
//导入axios
import axios from 'axios'
import Header from "@/components/Header/Header.vue";
import Goods from '@/components/Goods/Goods.vue'
import Footer from '@/components/Footer/Footer.vue'
import bus from '@/components/eventBus.js'
import Counter from '@/components/Counter/Counter.vue'
export default {
  components: {
    Header,
    Goods,
    Footer,
    Counter
  },
  data() {
    return {
      list:[],
    }
  },
  methods: {
    async toGetData(){
      const {data:res}=await axios.get('https://applet-base-api-t.itheima.net/api/cart')
      console.log(res);
      if(res.status===200){
        this.list=res.list;
      }
    },
    getNewState(val){
      this.list.some(item=>{
        if(item.goods_id===val.goods_id){
          item.goods_state=val.value;
          return true;
        }
      })
    },
    getFooterStateChange(val){
      console.log(val);
      this.list.forEach(item=>{
        item.goods_state=val;
      })
    },
    getNewCount(item,val){
      item.goods_count=val
    }
  },
  computed: {
    fullChecked(){
      return this.list.every(item=>item.goods_state===true)
    },
    awt(){
      return this.list.filter(item=>item.goods_state===true).reduce((total,item)=>{
        return total+=item.goods_count*item.goods_price
      },0)
    },
    allCount(){
      return this.list.filter(item=>item.goods_state).reduce((total,item)=>{
        return total+=item.goods_count;
      },0)
    }
  },
  created() {
    this.toGetData()
    // bus.$on('count-change',val=>{
    //   this.list.some(item=>{
    //     if(item.goods_id===val.id){
    //       item.goods_count=val.value;
    //       return true;
    //     }
    //   })
    // })
    
  },


}
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
