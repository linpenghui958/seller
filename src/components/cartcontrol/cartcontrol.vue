<template>
<div class="cartcontrol">
    <transition name="move">
      <div class="cart-decrease" v-show="food.count > 0"
      @click.stop.pervent = "decreaseCart($event)">
        <span class="inner iconfont icon-guanbi"></span>
      </div>
    </transition>

    <div class="cart-count" v-show="food.count > 0">{{food.count}}</div>
    <div class="cart-add iconfont icon-bofang" @click.stop.pervent="addCart($event)"></div>
</div>
</template>

<script type="text/ecmascript-6">
import Vue from 'vue';
export default {
    props: {
        food: {
        type: Object
        }
    },
    created() {

    },
    methods: {
      addCart(event) {
        if (!event._constructed) {
          return;
        }
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1);
        } else {
          this.food.count++;
        }
        this.$emit('cartadd', event.target);
      },
      decreaseCart(event) {
        if (!event._constructed) {
          return;
        }
        if (this.food.count) {
          this.food.count--;
        }
      }
    }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
.cartcontrol
  font-size: 0
  .cart-decrease
    display: inline-block
    padding: 6px
    font-size: 24px
    line-height: 24px
    color: rgb(0, 160, 220)
    .inner
      display: inline-block
      line-height: 24px
      font-size: 24px
      color: rgb(0, 160, 220)
      transform: rotate(0)
  .move-enter-active
    transition: all .5s ease
  .move-leave-active
    transition: all .5s ease
  .move-enter
    opacity: 0
    transform: translate3d(24px, 0 ,0) rotate(90deg)
  .move-leave-active
    transform: translate3d(24px, 0 ,0) rotate(90deg)
    opacity: 0
  .cart-count
    display: inline-block
    vertical-align: top
    width: 12px
    padding-top: 6px
    line-height: 24px
    text-aligh: center
    font-size: 10px
    color: rgb(147,153,159)
  .cart-add
    display: inline-block
    font-size: 24px
    line-height: 24px
    padding: 6px
    color: rgb(0, 160, 220)

</style>