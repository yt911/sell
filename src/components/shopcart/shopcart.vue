
<template>
  <div class="shopcart">
      <div class="content">
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
          <div class="content-right">
              <div class="pay" :class="payClass">{{payDesc}}</div>
          </div>
      </div>
  </div>
</template>

<script>
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
            default () {
                return [
                    {
                        price:28,
                        count:1
                    }
                ];
            }
        }
    },
    computed: {
        totalPrice () {
            let total = 0;
            this.selectFoods.forEach((food) => {
                total += food.price * food.count;
                
            });
            return total;
        },
        totalCount () {
            let count = 0;
            this.selectFoods.forEach((food) => {
                count += food.count;
            });
            return count;
        },
        choosePic () {
            let url;
            return url = '(totalCount>0)'?'../static/cart1.png':'../static/cart2.png';
        },
        payDesc () {
            if(this.totalPrice === 0) {
                return `￥${this.minPrice}起送`
            }else if(this.totalPrice <this.minPrice ){
                let diff = this.minPrice - this.totalPrice;
                return `还差${diff}元配送`;
            }else{
                return "去结算";
            }
        },
        payClass () {
            if(this.totalPrice < this.minPrice ){
                return 'no-enough';
            }else {
                return 'enough';
            }
        }
    }
}

</script>
<style scoped>
.shopcart{
    width: 100%;
    height: 48px;
    position: fixed;
    bottom: 0;
    left: 0;
    z-index: 50;
}
.content{
    display: flex;
    background: #141d27;
    font-size: 0;
}
.content-left{
    flex: 1;
}
.logo-wrapper{
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
.logo .icon{
    display:inline-block;
    width: 44px;
    height: 44px;
    background-color: #2b343c;
    border-radius: 50%;
}
.logo .highlight{
    background: rgb(0,160,220);
}
.logo .icon img{
    width: 26px;
    height: 26px;
    margin-left: 9px;
    margin-top: 9px;
}
.logo-wrapper .num{
    position: absolute;
    top: 0;
    right: 0;
    width: 24px;
    height: 16px;
    line-height: 16px;
    text-align: center;
    border-radius: 16px;
    font-size:9px;
    font-weight: 700;
    color:#fff;
    background: rgb(240,20,20);
    box-shadow: 0 4px 8px 0 rgba(0,0,0,0.4);

}
.price{
    display: inline-block;
    font-size: 16px;
    vertical-align: top;
    line-height: 24px;
    font-weight: 700;
    margin-top: 12px;
    box-sizing: border-box;
    padding-right: 12px;
    border-right: 1px solid rgba(255,255,255,0.1);
    color:rgba(255,255,255,0.4);
}
.highlightPrice{
    color: #fff;
}
.desc{
    display: inline-block;
    font-size: 10px;
    vertical-align: top;
    line-height: 24px;
    margin-top: 12px;
    margin-left: 12px;
    box-sizing: border-box;
    padding-right: 12px;
    color:rgba(255,255,255,0.4);
}
.content-right{
    flex: 0 0 105px;
    width: 105px;
}
.pay{
    height: 48px;
    line-height: 48px;
    text-align: center;
    font-size: 12px;
    color: rgba(255,255,255,0.4);
    font-weight: 700;
    background: #2b333b;
}
.not-enough{
    background: #2b333b;
}
.enough{
    background: #00b43c;
    color: #fff;
}
</style>