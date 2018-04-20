<!--  -->
<template>
  <div class="seller" ref="seller">
    <div class="content-wrapper">
      <div class="content-top">
        <div class="inner-top border-one-px">
          <h1 class="title">{{seller.name}}</h1>
          <div class="star-wrapper">
            <star :size="36" :score="seller.score"></star>
            <span class="ratingCount">({{seller.ratingCount}})</span>
            <span class="sellCount">{{seller.sellCount}}</span>
          </div>
          <div class="collect" @click="toggleFavorite">
            <img v-show="!this.favorite" src="../../../static/sck.png" alt="">
            <img v-show="this.favorite" src="../../../static/scm.png" alt="">
            <p>{{this.favoriteText}}</p>
          </div>
        </div>
        <div class="inner-bottom">
          <div class="item">
            <div class="text">起送价</div>
            <div class="num-box"><span class="num">{{seller.minPrice}}</span>元</div>
          </div>
          <div class="item">
            <div class="text">商家配送</div>
            <div class="num-box"><span class="num">{{seller.deliveryPrice}}</span>元</div>
          </div>
          <div class="item">
            <div class="text">平均配送时间</div>
            <div class="num-box"><span class="num">{{seller.deliveryTime}}</span>分钟</div>
          </div>
        </div>
      </div>
      <split></split>
      <div class="bulletin-wrapper">
        <h1 class="title">公告与活动</h1>
        <p>{{seller.bulletin}}</p>
        <ul v-if="seller.supports" class="supports">
          <li v-for="(support,index) in seller.supports" :key="support.id" class="support-item">
            <span class="icon" :class="classMap[seller.supports[index].type]"></span>
            <span class="text">{{seller.supports[index].description}}</span>
          </li>
        </ul>
      </div>
      <split></split>
      <div class="pics" v-show="seller.pics && seller.pics.length">
        <h1 class="title">商家实景</h1>
        <div class="pic-wrapper" ref="picWrapper">
          <ul class="pic-ul" ref="picUl">
            <li class="pic-item" v-for="pic in seller.pics" :key="pic.id">
              <img :src="pic" alt="" width="120" height="90">
            </li>
          </ul>
        </div>
      </div>
      <div class="info-wrapper">
        <h1 class="title">商家信息</h1>
        <ul>
          <li class="info-item" v-for="info in seller.infos" :key="info.id">{{info}}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import BScroll from 'better-scroll';
import star from 'components/star/star';
import split from 'components/split/split';
export default {
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
      favorite: true
    }
  },
  computed: {
    favoriteText () {
      return this.favorite ? '已收藏' : '收藏';
    }
  },
  created () {
    this.classMap = ["decrease", "discount", "special", "invoice", "guarantee"];
  },
  watch: {
    'seller' () {
      this.$nextTick(() => {
        this._initScroll();
        this._initPicScroll();
      })
    }
  },
  mounted () {
    this.$nextTick(() => {
      this._initScroll();
      this._initPicScroll();
    })
  },
  methods: {
    _initScroll () {
      if (!this.scroll) {
        this.scroll = new BScroll(this.$refs.seller, {
          click: true
        });
      } else {
        this.scroll.refresh();
      }
    },
    _initPicScroll () {
      if (this.seller.pics) {
        let picWidth = 120;
        let margin = 6;
        let width = (picWidth + margin) * this.seller.pics.length - margin;
        this.$refs.picUl.style.width = width + 'px';
        if (!this.picScroll) {
          this.picScroll = new BScroll(this.$refs.picWrapper, {
            scrollX: true,
            click: true,
            eventPassthrough: 'vertical'
          });
        } else {
          this.picScroll.refresh();
        }
      }
    },
    toggleFavorite (event) {
      if (!event._constructed) {
        return;
      }
      this.favorite = !this.favorite;
    }
  },
  components: {
    star,
    split
  }
};
</script>
<style scoped>
.seller{
  position: absolute;
  top: 174px;
  left: 0;
  bottom: 0;
  width: 100%;
  overflow: hidden;
  z-index: 30;
}
.content-top{
  margin: 0 18px;
  padding: 18px 0;
}
.inner-top{
  position: relative;
  padding-bottom: 18px;
}
h1{
  font-size: 14px;
  color: rgb(7,17,27);
  line-height: 14px;
  margin-bottom: 8px;
}
.star-wrapper{
  font-size: 0;
}
.star{
  display: inline-block;
  margin-right: 8px;
  vertical-align: top;
}
.ratingCount,.sellCount{
  font-size: 10px;
  color: rgb(77,85,93);
  line-height: 18px;
}
.ratingCount{
  margin-right: 12px;
}
.inner-bottom{
  display: flex;
  padding: 18px 0;
}
.inner-bottom .item{
  flex: 1;
  box-sizing: border-box;
  border-right: 1px solid rgba(7,17,27,0.1);
  text-align: center;
}
.inner-bottom .item .text{
  font-size: 10px;
  color: rgb(147,153,159);
  line-height: 10px;
  margin-bottom: 4px;
}
.inner-bottom .item .num{
  font-size: 10px;
  line-height: 24px;
  color:rgb(7,17,27);
}
.inner-bottom .item .num-box .num{
  font-size: 24px;
}
.inner-bottom .item:last-child{
  border: none;
}
.bulletin-wrapper{
  padding:18px 18px 0 18px;
}
.bulletin-wrapper p{
  font-size: 12px;
  color: rgb(240,20,20);
  line-height: 20px;
  padding: 0 12px;
  margin-bottom:16px;
}
.supports .support-item{
  padding: 16px 12px;
  font-size: 0;
  border-top: 1px solid rgba(7,17,27,0.1);
}
.supports .support-item .icon{
  display: inline-block;
  width: 16px;
  height: 16px;
  vertical-align: top;
  margin-right: 16px;
  background-size: 16px 16px;
  background-repeat: no-repeat;
}
.supports .decrease{
  background-image: url("decrease_2@2x.png");
}
.supports .discount{
  background-image: url("discount_2@2x.png");
}
.supports .guarantee{
  background-image: url("guarantee_2@2x.png");
}
.supports .invoice{
  background-image: url("invoice_2@2x.png");
}
.supports .special{
  background-image: url("special_2@2x.png");
}
.supports .text{
  font-size: 12px;
  line-height: 16px;
}
.pics{
  padding: 18px;
}
.pic-wrapper{
  width: 100%;
  overflow: hidden;
}
.pic-ul{
  font-size: 0;
}
.pic-item{
  display: inline-block;
  margin-left: 6px;
  width: 120px;
  height: 90px;
}
.pic-item:first-child{
  margin-left: 0;
}
.info-wrapper{
  padding: 18px;
}
.info-item{
  padding: 16px 12px;
  font-size: 12px;
  color: rgb(7,17,27);
  line-height: 16px;
  border-top: 1px solid rgba(7,17,27,0.1);
}
.collect{
  position: absolute;
  top: 0;
  right: 0;
  width: 60px;
  text-align: center;
}
.collect p{
  margin-top: 1px;
  font-size: 10px;
  color: rgb(77,85,93);
}
@media (-webkit-min-device-pixel-ratio: 3), (min-device-pixel-ratio: 3) {
  .supports .decrease{
    background-image: url("decrease_2@3x.png");
  }
  .supports .discount{
    background-image: url("discount_2@3x.png");
  }
  .supports .guarantee{
    background-image: url("guarantee_2@3x.png");
  }
  .supports .invoice{
    background-image: url("invoice_2@3x.png");
  }
  .supports .special{
    background-image: url("special_2@3x.png");
  }
}
</style>
