<template>
  <div class="home">
    <!-- 头部图片 -->
    <header class="header">
      <img src="../../assets/home.jpg" alt="">
    </header>
    <!-- 内容区域 -->
    <article>

      <!-- 分类 6个 -->
      <van-grid :column-num="3">
        <van-grid-item
          v-for="value in categories"
          :key="value.id"
          :icon="value.icon"
          :text="value.name"
        />
      </van-grid>
      <!-- 产品 n个 -->
      <briup-product-item
      @click="toBuyHandler(p)"
        v-for="p in products"
        :key="p.id"
        :data="p">
      </briup-product-item>
    </article>
   
  </div>
</template>
<script>
import {get,post} from '../../http/axios'
import { stringify } from 'querystring';
//import {mapState, mapActions} from 'vuex'     状态机
export default {
  
  created(){
    //查询栏目信息
    this.loadCategories();
    //查询产品
    this.loadProducts();
  },
  data(){
    return {
      categories:[],
      products:[]
    }
  },
  methods:{
    toBuyHandler(p){
      // alert(JSON.stringify(p));
      //跳转到订单确认页面，并且携带数据p
      this.$router.push({
        path:"/manager/order_confirm",
        query:p
      })
    },
    //加载栏目信息
    loadCategories(){
      let url = "/category/findAll";
      get(url).then((response)=>{
        this.categories = response.data.slice(0,6);
      })
    },
    //加载产品信息
    loadProducts(){
      let url = "/product/query"
      let params = {page:0,pageSize:100}
      post(url,params).then((response)=>{
        this.products = response.data.list;
      })

    }
  }
}
</script>
<style scoped>
.home {
  padding-bottom: 50px;
}
.header {
  height: 300px;
  overflow: hidden;
}
.header img {
  width: 100%;
}
</style>