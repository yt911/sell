
<template>
    <div>
        <div class="shopcart">
            <div class="content" @click="toggleList">
                <div class="content-left">
                    <div class="logo-wrapper">
                        <div class="logo">
                            <span class="icon" :class="{'highlight':totalCount>0}"><img :src="choosePic" alt=""></span>
                        </div>
                        <div class="num" v-show="totalCount>0">{{totalCount}}</div>
                    </div>
                    <div class="price" :class="{'highlightPrice':totalPrice>0}">￥{{totalPrice}}元</div>
                    <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
                </div>
                <div class="content-right" @click.stop="pay">
                    <div class="pay" :class="payClass">{{payDesc}}</div>
                </div>
            </div>
            <div class="shopcart-list" v-show="listShow">
                <div class="list-header">
                    <h1 class="title">购物车</h1>
                    <span class="empty" @click="empty">清空</span>
                </div>
                <div class="list-content" ref="listContent">
                    <ul>
                        <li class="food border-one-px" v-for="food in selectFoods">
                            <span class="name">{{food.name}}</span>
                            <div class="price">
                                <span>￥{{food.price*food.count}}</span>
                            </div>
                            <div class="cartcontrol-wrapper">
                                <cartControl :food="food"></cartControl>
                            </div>
                        </li>
                    </ul>
                </div>
            </div>
        </div>
        <div class="list-mask" v-show="listShow" @click="hideList"></div>
    </div>
</template>

<script>
import BScroll from "better-scroll";
import cartControl from "components/cartcontrol/cartcontrol";
export default {
  props: {
    deliveryPrice: {
      type: Number,
      default: 0
    },
    minPrice: {
      type: Number,
      default: 0
    },
    selectFoods: {
      type: Array,
      default() {
        return [];
      }
    }
  },
  data() {
    return {
      fold: true
    };
  },
  computed: {
    totalPrice() {
      let total = 0;
      this.selectFoods.forEach(food => {
        total += food.price * food.count;
      });
      return total;
    },
    totalCount() {
      let count = 0;
      this.selectFoods.forEach(food => {
        count += food.count;
      });
      return count;
    },
    choosePic() {
      let url;
      return (url = "(totalCount>0)"
        ? "../static/cart1.png"
        : "../static/cart2.png");
    },
    payDesc() {
      if (this.totalPrice === 0) {
        return `￥${this.minPrice}起送`;
      } else if (this.totalPrice < this.minPrice) {
        let diff = this.minPrice - this.totalPrice;
        return `还差${diff}元配送`;
      } else {
        return "去结算";
      }
    },
    payClass() {
      if (this.totalPrice < this.minPrice) {
        return "no-enough";
      } else {
        return "enough";
      }
    },
    listShow() {
      if (!this.totalCount) {
        this.fold = true;
        return false;
      }
      let show = !this.fold;
      if (show) {
        this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BScroll(this.$refs.listContent, {
              click: true
            });
          } else {
            this.scroll.refresh();
          }
        });
      }
      return show;
    }
  },
  methods: {
    toggleList() {
      if (!this.totalCount) {
        return;
      }
      this.fold = !this.fold;
    },
    empty() {
      this.selectFoods.forEach(food => {
        food.count = 0;
      });
    },
    hideList() {
      this.fold = true;
    },
    pay() {
      if (this.totalPrice < this.minPrice) {
        return;
      }
      window.alert(`支付${this.totalPrice}元`);
    }
  },
  components: {
    cartControl
  }
};
</script>
<style scoped>
.shopcart {
  width: 100%;
  height: 48px;
  position: fixed;
  bottom: 0;
  left: 0;
  z-index: 50;
}
.content {
  display: flex;
  background: #141d27;
  font-size: 0;
}
.content-left {
  flex: 1;
}
.logo-wrapper {
  display: inline-block;
  position: relative;
  top: -10px;
  margin: 0 12px;
  padding: 6px;
  width: 56px;
  height: 56px;
  box-sizing: border-box;
  vertical-align: top;
  border-radius: 50%;
  background: #141d27;
}
.logo .icon {
  display: inline-block;
  width: 44px;
  height: 44px;
  background-color: #2b343c;
  border-radius: 50%;
}
.logo .highlight {
  background: rgb(0, 160, 220);
}
.logo .icon img {
  width: 26px;
  height: 26px;
  margin-left: 9px;
  margin-top: 9px;
}
.logo-wrapper .num {
  position: absolute;
  top: 0;
  right: 0;
  width: 24px;
  height: 16px;
  line-height: 16px;
  text-align: center;
  border-radius: 16px;
  font-size: 9px;
  font-weight: 700;
  color: #fff;
  background: rgb(240, 20, 20);
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.4);
}
.price {
  display: inline-block;
  font-size: 16px;
  vertical-align: top;
  line-height: 24px;
  font-weight: 700;
  margin-top: 12px;
  box-sizing: border-box;
  padding-right: 12px;
  border-right: 1px solid rgba(255, 255, 255, 0.1);
  color: rgba(255, 255, 255, 0.4);
}
.highlightPrice {
  color: #fff;
}
.desc {
  display: inline-block;
  font-size: 10px;
  vertical-align: top;
  line-height: 24px;
  margin-top: 12px;
  margin-left: 12px;
  box-sizing: border-box;
  padding-right: 12px;
  color: rgba(255, 255, 255, 0.4);
}
.content-right {
  flex: 0 0 105px;
  width: 105px;
}
.pay {
  height: 48px;
  line-height: 48px;
  text-align: center;
  font-size: 12px;
  color: rgba(255, 255, 255, 0.4);
  font-weight: 700;
  background: #2b333b;
}
.not-enough {
  background: #2b333b;
}
.enough {
  background: #00b43c;
  color: #fff;
}
.shopcart-list {
  position: absolute;
  bottom: 48px;
  left: 0;
  z-index: -1;
  width: 100%;
}
.list-header {
  height: 40px;
  line-height: 40px;
  padding: 0 18px;
  background: #f3f5f7;
  border-bottom: 1px solid rgba(7, 17, 27, 0.1);
}
.list-header .title {
  float: left;
  font-size: 14px;
  color: rgb(7, 17, 27);
  font-weight: 200;
}
.list-header .empty {
  float: right;
  font-size: 12px;
  color: rgb(0, 160, 220);
}
.list-content {
  padding: 0 18px;
  max-height: 192px;
  overflow: hidden;
  background: #fff;
}
.list-content .food {
  position: relative;
  padding: 12px 0;
  box-sizing: border-box;
}
.list-content .name {
  font-size: 14px;
  line-height: 24px;
  color: rgb(7, 17, 27);
}
.list-content .price {
  position: absolute;
  right: 90px;
  bottom: 12px;
  line-height: 24px;
  font-size: 24px;
  color: rgb(240, 20, 20);
  font-weight: 700;
}
.list-content .cartcontrol-wrapper {
  position: absolute;
  right: 0;
  bottom: 6px;
}
.list-mask {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 40;
  background: rgba(7, 17, 27, 0.6);
}
</style>