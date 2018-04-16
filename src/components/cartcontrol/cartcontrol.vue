
<template>
  <div class="cartcontrol">
      <div class="cart-decrease" v-show="food.count>0" @click.stop="decreaseCart($event)"><img src="./../../../static/jh.png" alt=""></div>
      <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
      <div class="cart-add" @click.stop="addCart($event)"><img src="./../../../static/ah.png" alt=""></div>
  </div>
</template>

<script>
import Vue from "vue";
export default {
  props: {
    food: {
      type: Object
    }
  },
  methods: {
    addCart (event) {
      if (!event._constructed) {
        return;
      }
      if (!this.food.count) {
        Vue.set(this.food, "count", 1);
      } else {
        this.food.count++;
      }
    },
    decreaseCart (event) {
      if (!event._constructed) {
        return;
      }
      if (this.food.count > 0) {
        this.food.count--;
      }
    }
  }
};
</script>
<style scoped>
.cartcontrol {
  font-size: 0;
}
.cart-decrease {
  display: inline-block;
  padding: 6px;
}
.cart-count {
  display: inline-block;
  font-size: 10px;
  vertical-align: top;
  padding-top: 4px;
  line-height: 24px;
  width: 12px;
  color: rgb(147, 153, 159);
}
.cart-add {
  display: inline-block;
  padding: 6px;
}
.cartcontrol img {
  width: 20px;
}
</style>
