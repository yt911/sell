<template>
  <div>
       <v-header :seller="seller"></v-header>
       <div class="tab border-one-px ">
           <div class="tab-item">
             <router-link :to="{path: 'goods'}">商品</router-link>
           </div>
           <div class="tab-item">
             <router-link :to="{path: 'ratings'}">评论</router-link>
             <!-- <a v-link="{path:'/ratings'}">评论</a> -->
           </div>
           <div class="tab-item">
             <router-link :to="{path: 'seller'}">商家</router-link>
             <!-- <a v-link="{path:'/seller'}">商家</a> -->
           </div>
       </div>
       <router-view :seller="seller"></router-view>
  </div>
</template>

<script>
import header from "./components/header/header.vue";
import goods from "./components/goods/goods.vue";
export default {
  data() {
    return {
      seller: {}
    };
  },
  created() {
    this.$http.get("/api/seller").then(response => {
      response = response.body;
      if (response.errno === 0) {
        this.seller = response.data;
      }
    });
  },
  components: {
    "v-header": header
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.tab {
  display: flex;
  width: 100%;
  height: 40px;
  line-height: 40px;
  /* border-bottom: 1px solid rgba(7,17,27,0.1); */
}
.tab .tab-item {
  flex: 1;
  text-align: center;
}
.tab a {
  display: block;
  font-size: 14px;
  color: rgb(77, 85, 93);
}
.tab a.active {
  color: rgb(240, 20, 20);
}
</style>
