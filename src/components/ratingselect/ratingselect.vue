<!--  -->
<template>
  <div class="ratingselect">
     <div class="rating-type border-one-px">
         <span class="block positive" @click="select(2,$event)" :class="{'active':selectType===2}">{{desc.all}}<span class="count">{{ratings.length}}</span></span>
         <span class="block positive" @click="select(0,$event)" :class="{'active':selectType===0}">{{desc.positive}}<span class="count">{{positive.length}}</span></span>
         <span class="block negative" @click="select(1,$event)" :class="{'active':selectType===1}">{{desc.negative}}<span class="count">{{negative.length}}</span></span>
     </div>
     <div class="switch">
         <span @click="toggleContent($event)"><img :src="ifContentPic" alt=""></span>
         <span class="text">只看有内容的评价</span>
     </div> 
  </div>
</template>

<script>
const POSITIVE = 0;
const NEGATIVE = 1;
const ALL = 2;
export default {
    props: {
        ratings: {
            type: Array,
            default () {
                return []
            }
        },
        desc: {
            type: Object,
            default () {
                return {
                    all: '全部',
                    positive: '满意',
                    negative: '吐槽'
                }
            }
        },
        onlyContent: {
            type: Boolean,
            default: false
        },
        selectType: {
            type: Number,
            default: ALL
        },
    },
    data () {
        return {
            content: this.onlyContent
        }
    },
    computed: {
        ifContentPic () {
            return this.onlyContent ? '../static/ls.png':'../static/hs.png';
        },
        _onlyContent () {
             if(this.onlyContent){
                 return false
             }else{
                 return true
             }
        },
        positive () {
            return this.ratings.filter((rating) => {
                return rating.rateType === POSITIVE;
            })
        },
        negative () {
            return this.ratings.filter((rating) => {
                return rating.rateType === NEGATIVE;
            })
        }
    },
    methods: {
        toggleContent (event) {
            if(!event._constructed){
                return;
            }
            
            this.$emit('toggleContent',this._onlyContent);
        },
        select (type,event) {
            if(!event._constructed){
                return;
            }
            this.$emit('select',type);
        }
    }
}

</script>
<style>
.rating-type{
    padding: 18px 0;
    margin: 0 18px;
}
.rating-type .block{
    display: inline-block;
    font-size: 12px;
    line-height: 16px;
    padding: 8px 12px;
    margin-right: 8px;
    border-radius: 2px;
    color: rgb(77,85,93);
}
.rating-type .block.active{
    color: #fff;
}
.block .count{
    font-size: 8px;
    margin-left: 2px;
    line-height: 16px;
}
.positive{
    background: rgba(0,160,220,0.2);
}
.positive.active{
    background: rgb(0,160,220);
}
.negative{
    background: rgba(77,85,93,0.2);
}
.negative.active{
    background: rgb(77,85,93);
}
.switch{
    padding: 12px 18px;
    border-bottom: 1px solid rgba(7,17,27,0.1);
    line-height: 24px;
    font-size: 0;
    color: rgb(147,153,159);
}
.switch span{
    display: inline-block;
}
.switch img{
    width: 16px;
    vertical-align: top;
}
.switch .text{
    font-size: 12px;
    line-height: 24px;
}
</style>