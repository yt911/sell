
<template>
  <div class="food" v-show="showFlag" ref="food">
      <div class="food-content">
          <div class="image-header">
              <img :src="food.image" alt="">
              <div class="back" @click="hide">&lt;</div>
          </div>
          <div class="content">
              <h1 class="title">{{food.name}}</h1>
              <div class="detail">
                  <span class="sell-count">月售{{food.sellCount}}份</span>
                  <span class="rating">好评率{{food.rating}}%</span>
              </div>
              <div class="price">
                  <span class="now">￥{{food.price}}</span>
                  <span v-show="food.oldPrice" class="old">￥{{food.oldPrice}}</span>
              </div>
               <div class="cartcontrol-wrapper">
                <cartControl :food="food"></cartControl>
            </div>
            <div class="buy" v-show="!food.count || food.count==0" @click.stop="addFirst($event)">加入购物车</div>
          </div>
          <split v-show="food.info"></split>
          <div class="info">
              <div class="title">商品信息</div>
              <div class="text" v-show="food.info">{{food.info}}</div>
          </div>
          <split></split>
          <div class="rating">
              <h1 class="title">商品评价</h1>
              <ratingSelect  @toggleContent="togglecontent"  @select="selectRating"  :ratings="food.ratings" :select-type="selectType" :only-content="onlyContent" :desc="desc"></ratingSelect>
              <div class="rating-wrapper">
                  <ul v-show="food.ratings && food.ratings.length">
                      <li v-show="needShow(rating.rateType,rating.text)" v-for="rating in food.ratings" :key="rating.id" class="rating-item border-one-px">
                          <div class="user">
                              <span class="name">{{rating.username}}</span>
                              <img :src="rating.avatar" alt="" class="avatar" width="12" height="12">
                          </div>
                          <div class="time">{{rating.rateTime}}</div>
                          <p class="text">
                              <span class="icon"><img v-show="rating.rateType==0" src="./../../../static/dzu.png" alt=""><img v-show="!rating.rateType==0" src="./../../../static/dzut.png" alt=""></span>{{rating.text}}
                          </p>
                      </li>
                  </ul>
                  <div class="no-rating" v-show="!food.ratings || !food.ratings.length">暂无评价</div>
              </div>
          </div>
      </div>
  </div>
</template>

<script>
import Vue from 'vue'
import BScroll from 'better-scroll'
import cartControl from 'components/cartcontrol/cartcontrol'
import split from 'components/split/split'
import ratingSelect from 'components/ratingselect/ratingselect'
// const POSITIVE = 0
// const NEGATIVE = 1
const ALL = 2
export default {
  props: {
    food: {
      type: Object
    }
  },
  data () {
    return {
      showFlag: false,
      selectType: ALL,
      onlyContent: false,
      desc: {
        all: '全部',
        positive: '推荐',
        negative: '吐槽'
      }
    }
  },
  computed: {},
  methods: {
    show () {
      this.showFlag = true
      this.selectType = ALL
      this.onlyContent = false
      this.$nextTick(() => {
        if (!this.BScroll) {
          this.scroll = new BScroll(this.$refs.food, {
            click: true
          })
        } else {
          this.scroll.refresh()
        }
      })
    },
    hide () {
      this.showFlag = false
    },
    addFirst (event) {
      if (!event._constructed) {
        return
      }
      Vue.set(this.food, 'count', 1)
    },
    selectRating (type) {
      this.selectType = type
      this.$nextTick(() => {
        this.scroll.refresh()
      })
    },
    togglecontent (onlyContent) {
      this.onlyContent = onlyContent
      this.$nextTick(() => {
        this.scroll.refresh()
      })
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
    cartControl,
    split,
    ratingSelect
  }
}
</script>
<style scoped>
.food {
  position: fixed;
  width: 100%;
  top: 0;
  left: 0;
  bottom: 48px;
  z-index: 30;
  background: #fff;
}
.image-header {
  position: relative;
  width: 100%;
  height: 0;
  padding-top: 100%;
}
.image-header img {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
.image-header .back {
  position: absolute;
  top: 20px;
  left: 20px;
  font-size: 26px;
  color: #fff;
}
.content {
  position: relative;
  padding: 18px;
}
.content .title {
  font-size: 14px;
  font-weight: 700;
  color: rgb(7, 17, 27);
  line-height: 20px;
  margin-bottom: 8px;
}
.content .detail {
  margin: 0;
  margin-bottom: 18px;
  font-size: 0px;
  height: 10px;
  color: rgb(147, 153, 159);
  line-height: 10px;
}
.content .detail span {
  font-size: 10px;
  color: rgb(147, 153, 159);
  line-height: 10px;
}
.sell-count {
  margin-right: 12px;
}
.content .price {
  font-weight: 700;
  line-height: 24px;
}
.content .now {
  font-size: 14px;
  color: rgb(240, 20, 20);
  margin-right: 8px;
}
.content .old {
  font-size: 10px;
  color: rgb(147, 153, 159);
  text-decoration: line-through;
}
.cartcontrol-wrapper {
  position: absolute;
  right: 12px;
  bottom: 12px;
}
.buy {
  position: absolute;
  right: 18px;
  bottom: 18px;
  z-index: 10;
  line-height: 24px;
  font-size: 10px;
  color: #fff;
  width: 74px;
  height: 24px;
  border-radius: 12px;
  background: rgb(0, 160, 220);
  box-sizing: border-box;
  text-align: center;
}
.info {
  padding: 18px;
}
.info .title {
  font-size: 14px;
  color: rgb(7, 17, 27);
  line-height: 20px;
  margin-bottom: 8px;
}
.info .text {
  padding: 0 8px;
  font-size: 12px;
  font-weight: 200;
  color: rgb(77, 85, 93);
  line-height: 24px;
  margin-bottom: 6px;
}
.rating {
  padding-top: 18px;
}
.rating .title {
  font-size: 14px;
  color: rgb(7, 17, 27);
  line-height: 20px;
  margin-left: 18px;
}
.rating-wrapper {
  padding: 0 18px;
}
.rating-item {
  position: relative;
  padding: 16px 0;
}
.rating-item .user {
  position: absolute;
  right: 0;
  top: 16px;
  font-size: 0;
  line-height: 12px;
}
.user .name {
  display: inline-block;
  vertical-align: middle;
  padding: 6px;
  font-size: 10px;
  color: rgb(147, 153, 159);
  margin-right: 6px;
}
.user .avatar {
  border-radius: 50%;
}
.time {
  line-height: 12px;
  font-size: 10px;
  color: rgb(147, 153, 159);
  margin-right: 6px;
}
.rating-item .text {
  line-height: 16px;
  font-size: 12px;
  color: rgb(7, 17, 27);
  margin-top: 8px;
}
.rating-item .text .icon {
  margin-right: 4px;
  width: 12px;
}
.no-rating {
  padding: 16px 0;
  font-size: 12px;
  color: rgb(7, 17, 27);
}
</style>
