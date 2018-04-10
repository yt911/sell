<!--  -->
<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
        <li v-for="(good,index) in goods" class="menu-item" @click="selectMenu(index,$event)" :class="{'current':currentIndex===index}">
          <span class="text border-one-px"><span class="icon" v-show="good.type>0" :class="classMap[good.type]"></span>{{good.name}}</span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foodsWrapper">
      <ul>
        <li v-for="(good,index) in goods" class="food-list food-list-hook">
          <h1 class="title">{{good.name}}</h1>
          <ul>
            <li v-for="food in good.foods" class="food-item border-one-px">
              <div class="icon">
                <img :src="food.icon" alt="">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span class="count">月售{{food.sellCount}}份</span>
                  <span>好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="now">￥{{food.price}}</span>
                  <span v-show="food.oldPrice" class="old">￥{{food.oldPrice}}</span>
                </div>
                <div class="cartcontrol-wrapper">
                  <cartControl :food="food"></cartControl>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shopcart :select-foods="selectFoods" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopcart>
  </div>
</template>

<script>
import BScroll from 'better-scroll';
import shopcart from 'components/shopcart/shopcart';
import cartControl from 'components/cartcontrol/cartcontrol';
export default {
  data (){
    return {
      goods: [],
      listHeight: [],
      scrollY: 0
    }
  },
  props: {
    seller: {     
    }
  },
  computed: {
    currentIndex () {
      for(let i=0;i<this.listHeight.length;i++){
        let height1 = this.listHeight[i];
        let height2 = this.listHeight[i+1];
        if(!height2 || (this.scrollY>=height1 && this.scrollY<height2)){
          return i;
        }
      }
      return 0;
    },
    selectFoods () {
      let foods = [];
      this.goods.forEach((good) => {
        good.foods.forEach((food) => {
          if (food.count) {
            foods.push(food);
          }
        })
      });
      return foods;
    }
    
  },
  created (){
    this.classMap = ["decrease", "discount", "special", "invoice", "guarantee"];    
    this.$http.get('/api/goods').then(response => {
      response = response.body;
      if (response.errno === 0) {
        this.goods = response.data;
        this.$nextTick(() => {
          this._initScroll();
          this._canculateHeight();
        });
      }
    })
  },
  methods: {
    _initScroll () {
      this.menuScroll = new BScroll(this.$refs.menuWrapper,{
        click: true
      });
      this.foosScroll = new BScroll(this.$refs.foodsWrapper,{
        probeType: 3,
        click: true
      });
      this.foosScroll.on('scroll',(pos) => {
        this.scrollY = Math.abs(Math.round(pos.y));
      })
    },
    _canculateHeight () {
      let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
      let height = 0;
      this.listHeight.push(height);
      for(let i=0;i<foodList.length;i++){
        var item = foodList[i];
        height += item.clientHeight;
        this.listHeight.push(height);
      }
    },
    selectMenu (index,event) {
      if(!event._constructed){
        return;
      }
      let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
      let el = foodList[index];
      this.foosScroll.scrollToElement(el,300);
      console.log(index);

    }
  },
  components: {
    shopcart,
    cartControl
  }
};
</script>
<style scoped>
.goods{
  display: flex;
  position: absolute;
  top: 174px;
  bottom: 46px;
  width: 100%;
  overflow: hidden;
}
.menu-wrapper{
  flex: 0 0 80px;
  width: 80px;
  background: #f3f5f7;
}
.menu-item{
  width:56px; 
  height: 54px;
  line-height: 14px;
  display: table;
  padding:0 12px;
  margin: 0 auto;
}
.current{
  position: relative;
  margin-top: -1px;
  z-index: 10;
  background: #fff;
  font-weight: 700;
}
.menu-item .text{
  display: table-cell;
  vertical-align: middle;
  width: 56px;
  font-size: 12px;
}
.current .text{
  border: none;
}
.menu-wrapper .icon {
  display: inline-block;
  width: 12px;
  height: 12px;
  margin-right: 2px;
  background-size: 12px 12px;
  background-repeat: no-repeat;
  vertical-align: top;
}
.decrease {
  background-image: url("decrease_3@2x.png");
}
.discount {
  background-image: url("discount_3@2x.png");
}
.guarantee {
  background-image: url("guarantee_3@2x.png");
}
.invoice {
  background-image: url("invoice_3@2x.png");
}
.special {
  background-image: url("special_3@2x.png");
}
.foods-wrapper{
  flex: 1
}
.food-list .title{
  padding-left: 14px;
  height: 26px;
  line-height: 26px;
  font-size: 12px;
  color: rgb(147,153,159);
  background: #f3f5f7;
  border-left: 2px solid #d9dde1;
}
.food-item{
  display: flex;
  margin:18px;
  padding-bottom: 18px;
}
.food-item:last-child{
  border: none;
  margin-bottom: 18px;
}
.food-item .icon{
 flex: 0,0,57px; 
 margin-right:10px;
}
.food-item .icon img{
  width: 57px; 
}
.food-item .content{
  flex: 1;
}
.food-item .name{
  font-size: 14px;
  margin: 2px 0 8px 0;
  line-height: 14px;
  color: rgb(7,17,27);
}
.food-item .desc{
  margin: 0;
  margin-bottom: 8px;
  font-size: 10px;
  color: rgb(147,153,159);
  line-height: 12px;
}
.food-item .extra span{
  font-size: 10px;
  color: rgb(147,153,159);
  line-height: 10px;
}
.food-item .extra .count{
  margin-right: 12px; 
}
.food-item .price{
  font-weight: 700;
  line-height: 24px;
}
.food-item .now{
  font-size: 14px;
  color:rgb(240,20,20);
  margin-right: 8px;
}
.food-item .old{
  font-size: 10px;
  color:rgb(147,153,159);
  text-decoration: line-through;
}
.cartcontrol-wrapper{
  position: absolute;
  right: 0;
  bottom: 12px;

}
@media (-webkit-min-device-pixel-ratio: 3), (min-device-pixel-ratio: 3) {
  .decrease {
    background-image: url("decrease_3@3x.png");
  }
  .discount {
    background-image: url("discount_3@3x.png");
  }
  .guarantee {
    background-image: url("guarantee_3@3x.png");
  }
  .invoice {
    background-image: url("invoice_3@3x.png");
  }
  .special {
    background-image: url("special_3@3x.png");
  }
}
  
</style>
