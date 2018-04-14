<!--  -->
<template>
  <div class="ratings">
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
    </div>
  </div>
</template>

<script>
import star from 'components/star/star';
import split from 'components/split/split';
export default {
  props:{
    seller: {
      type: Object
    }
  },
  data () {
    return {};
  },
  components:{
    star,
    split
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
