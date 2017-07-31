<template>
<div>
  <transition name="move">
    <div v-show="showFlag" class="food" ref="food">
      <div class="food-content">
        <div class="image-header">
          <img :src="food.image">
          <div class="back" @click="hide">
            <i class="iconfont icon-kuaitui"></i>
          </div>
        </div>
        <div class="foods-content">
          <h1 class="title">{{food.name}}</h1>
          <div class="detail">
            <span class="sell-count">月售{{food.count}}</span>
            <span class="rating">好评率{{food.rating}}%</span>
          </div>
          <div class="price">
            <span class="now">￥{{food.price}}</span>
            <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
          </div>
          <div class="cartcontrol-wrapper">
            <cartcontrol :food="food"></cartcontrol>
          </div>
           <div @click="addFirst" class="buy" v-show="!food.count || food.count === 0">加入购物车</div>
        </div>
        <split ></split>
        <div class="rating">
          <div class="title">商品评价</div>
          <ratingselect :select-type="selectType" :only-content="onlyContent" :desc="desc" :ratings="food.ratings"></ratingselect>

        </div>
      </div>
    </div>
  </transition>

</div>

</template>

<script type="text/ecmascript-6">
import BScroll from 'better-scroll';
import Vue from 'vue';
import cartcontrol from '../cartcontrol/cartcontrol';
import split from '../split/split';
import ratingselect from '../ratingselect/ratingselect';
import {formatDate} from '../../common/date';

/* eslint-disable no-unused-vars */
const POSITIVE = 0;
const NEGATIVE = 1;
const ALL = 2;

export default {
  props: {
    food: {
      type: Object
    }
  },
  data () {
    return {
      showFlag: false,
      selectType: 2,
      onlyContent: true,
      desc: {
        all: '全部',
        positive: '推荐',
        negative: '吐槽'
      }
    };
  },
  methods: {
    addFirst (event) {
      console.log('111');
      if (event._constructed) {
        return;
      }
      Vue.set(this.food, 'count', 1);
    },
    hide() {
      this.showFlag = false;
    },
    show() {
      this.showFlag = true;
      this.selectType = ALL;
      this.onlyContent = false;
      this.$nextTick(() => {
        if (!this.scroll) {
          this.scroll = new BScroll(this.$refs.food, {
            click: true
          });
        } else {
          this.scroll.refresh();
        }
      });
    }
  },
  filters: {
    formatDate(time) {
      let date = new Date(time);
      return formatDate(date, 'yyyy-MM-dd hh:mm');
    }
  },
  components: {
    cartcontrol,
    split,
    ratingselect
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .food
    position: fixed
    left: 0
    top: 0
    bottom: 48px
    z-index: 7
    width: 100%
    background: #fff
    transform: translate3d(0,0,0)
  .move-enter-active,.move-leave-active
    transition: all 0.7s
  .move-enter,.move-leave-active
    transform: translate3d(100%,0,0)
  .image-header
    position:relative
    width: 100%
    height: 0
    padding-bottom: 100%
    img
      position:absolute
      top: 0
      left: 0
      width: 100%
      height: 100%
    .back
      position:absolute
      top: 10px
      left: 0
      .icon-kuaitui
        display: block
        padding: 10px
        font-size: 20px
        color: #fff
  .foods-content
    position:relative
    padding: 18px
    .title
      line-height: 14px
      margin-bottom: 8px
      font-size: 14px
      font-weight: 700
      color: rgb(7, 17, 27)
    .detail
      margin-bottom: 18px
      line-height:10px
      font-size: 0
      .sell-count,.rating
        font-size: 10px
        color: rgb(147, 153, 159)
      .sell-count
        margin-right: 12px
      .price
        font-weight: 700
        line-height: 24px
        .now
          font-size: 14px
          margin-right: 8px
          color: rgb(240, 20, 20)
        .old
          text-decoration: line-through
          font-size: 10px
          color: rgb(147, 153, 159)
  .cartcontrol-wrapper
    position: absolute
    right: 12px
    bottom: 12px
  .buy
    position:absolute
    right: 18px
    bottom: 18px
    z-index: 10
    height: 24px
    line-height: 24px
    padding: 0 12px
    box-sizing: border-box
    font-size: 10px
    border-radius: 12px
    color: #fff
    background: rgb(0, 160, 220)
  .rating
    padding-top: 18px
    .title
      line-height: 14px
      margin-left: 18px
      font-size: 14px
      color: rgb(7, 17, 27)

</style>