<!--  -->
<template>
  <div class="ratings" ref="ratingsBox">
    <div class="ratings-content">
      <div class="overview">
        <div class="overview-left">
          <h1 class="score">{{seller.score}}</h1>
          <div class="title">综合评分</div>
          <div class="rank">高于周边商家{{seller.rankRate}}%</div>
        </div>
        <div class="overview-right">
          <div class="score-wrapper">
            <span class="title">服务态度</span>
            <star :size="36" :score="seller.serviceScore"></star>
            <span class="score">{{seller.serviceScore}}</span>
          </div>
          <div class="score-wrapper">
            <span class="title">商品评分</span>
            <star :size="36" :score="seller.foodScore"></star>
            <span class="score">{{seller.foodScore}}</span>
          </div>
          <div class="delivery-time">
            <span class="title">送达时间</span>
            <span class="time">{{seller.deliveryTime}}分钟</span>
          </div>
        </div>
      </div>
      <split></split>
      <ratingSelect  @toggleContent="togglecontent"  @select="selectRating"  :ratings="ratings" :select-type="selectType" :only-content="onlyContent" :desc="desc"></ratingSelect>
      <div class="rating-wrapper">
        <ul>
          <li v-for="rating in ratings" :key="rating.id" class="rating-item border-one-px" v-show="needShow(rating.rateType,rating.text)">
            <div class="avatar">
              <img :src="rating.avatar" alt="" width="28" height="28">
            </div>
            <div class="content">
              <h1 class="name">{{rating.username}}</h1>
              <div class="star-wrapper">
                <star :size="24" :score="rating.score"></star>
                <span class="delivery" v-show="rating.deliveryTime">{{rating.deliveryTime}}</span>
              </div>
              <p class="text">{{rating.text}}</p>
              <div class="recommend">
                <span class="icon"><img v-show="rating.rateType==0" src="./../../../static/dzu.png" alt=""><img v-show="!rating.rateType==0" src="./../../../static/dzut.png" alt=""></span>
                <span v-show="rating.recommend && rating.recommend.length" v-for="item in rating.recommend" :key="item.id" class="item">{{item}}</span>
              </div>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import BScroll from 'better-scroll';
import star from 'components/star/star';
import split from 'components/split/split';
import ratingSelect from 'components/ratingselect/ratingselect';
const ALL = 2;
export default {
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
      ratings: [],
      selectType: ALL,
      onlyContent: false,
      desc: {
        all: '全部',
        positive: '推荐',
        negative: '吐槽'
      }
    };
  },
  created () {
    this.$http.get('/api/ratings').then((reponse) => {
      reponse = reponse.body;
      if (reponse.errno === 0) {
        this.ratings = reponse.data;
        this.$nextTick(() => {
          this._isScroll();
        })
      }
    })
  },
  methods: {
    _isScroll () {
      this.ratingsBox = new BScroll(this.$refs.ratingsBox, {
        click: true
      });
    },
    selectRating (type) {
      this.selectType = type
      this.$nextTick(() => {
        this.ratingsBox.refresh()
      });
    },
    togglecontent (onlyContent) {
      this.onlyContent = onlyContent
      this.$nextTick(() => {
        this.ratingsBox.refresh()
      });
    },
    needShow (type, text) {
      if (this.onlyContent && !text) {
        return false
      }
      if (this.selectType === ALL) {
        return true
      } else {
        return type === this.selectType
      }
    }
  },
  components: {
    star,
    split,
    ratingSelect
  }
};
</script>
<style scoped>
.ratings{
  position: absolute;
  top: 174px;
  left: 0;
  bottom: 0;
  width: 100%;
  overflow: hidden;
}
.overview{
  display: flex;
  padding: 18px 0;
}
.overview-left{
  flex: 0 0 137px;
  width: 137px;
  text-align:center;
  border-right: 1px solid rgba(7,17,27,0.1);
  font-size: 0;
  padding: 6px 0;
}
.overview-right{
  flex:1;
  padding:6px 0 6px 24px;
}
.overview-left .score{
  font-size: 24px;
  color: rgb(255,153,0);
  line-height: 28px;
  margin-bottom: 6px;
}
.overview-left .title{
  font-size: 12px;
  color: rgb(7,17,27);
  line-height: 12px;
  margin-bottom:8px;
}
.overview-left .rank{
  font-size: 10px;
  color:rgb(147,152,159);
  line-height: 10px;
}
.score-wrapper{
  margin-bottom: 8px;
  font-size: 0;
}
.score-wrapper .title{
  font-size: 12px;
  color: rgb(7,17,27);
  line-height: 18px;
}
.score-wrapper .star{
  display: inline-block;
  margin:0 12px;
}
.score-wrapper .score{
  font-size: 12px;
  color:rgb(255,153,0);
  line-height: 18px;
}
.deliver-time{
  font-size: 0;
}
.delivery-time .title{
  font-size: 12px;
  color: rgb(7,17,27);
  line-height: 18px;
}
.delivery-time .time{
  font-size: 12px;
  color: rgb(147,152,159);
  line-height: 18px;
  margin-left: 12px;
}
.rating-wrapper{
  padding: 0 18px;
}
.rating-item{
  display: flex;
  padding: 18px 0;
  color: rgb(7,17,27);
}
.rating-item .avatar{
  flex: 0 0 28px;
  margin-right: 12px;
}
.rating-item .avatar img{
  border-radius: 50%;
}
.content{
  flex: 1;
  position: relative;
}
.content .name{
  line-height: 12px;
  font-size: 10px;
  color: rgb(7,17,27);
  margin-bottom: 4px;
}
.content .star-wrapper{
  margin-bottom: 6px;
  font-size: 0;
}
.content .star-wrapper .star{
  margin-right: 6px;
  display: inline-block;
}
.content .star-wrapper .delivery{
  font-size: 10px;
  color: rgb(147,152,159);
  line-height: 12px;
}
.content .text{
  line-height: 18px;
  font-size: 12px;
  color: rgb(7,17,27);
  margin-bottom: 8px;
}
.content .recommend{
  line-height: 16px;
}
.recommend .item{
  display: inline-block;
  margin: 0 8px 4px 0;
  border: 1px solid rgba(7,17,27,0.1);
  color: rgb(147,152,159);
  font-size: 12px;
  padding: 0 6px;
}
@media only screen and (max-width:320px){
  .overview-left{
    flex: 0 0 120px;
    width: 120px;
  }
  .overview-right{
    padding-left: 6px;
  }
}
</style>
