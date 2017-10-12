<template>
  <div class="cartcontrol">
    <transition name="move">
      <div class="cart-decrease" v-show="food.count>0" @click.stop.prevent="decreaseCart">
        <span class="inner icon-remove_circle_outline"></span>
      </div>
    </transition>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add icon-add_circle" @click.stop.prevent="addCart"></div>
  </div>
</template>

<script type="text/ecmascript-6">
  import Vue from 'vue'
  export default {
    props: {
      food: {
        type: Object
      }
    },
    methods: {
      addCart(event) {
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1)
          this.food.count = 1
        } else {
          this.food.count++
        }
        this.$emit('add', event.target)
      },
      decreaseCart() {
        if (this.food.count) {
          this.food.count--
        }
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .cartcontrol
    display: flex
    .cart-decrease
      &.move-enter-active,&.move-leave-active
        transition: all .4s linear
      .inner
        color: rgb(0,160,220)
        font-size: 24px
        line-height: 24px
        transform: rotate(0)
      &.move-enter,&.move-leave-to
        opacity: 0
        transform: translate3d(24px,0,0) rotate(180deg)
    .cart-add
      color: rgb(0,160,220)
      font-size: 24px
      line-height: 24px
    .cart-count
      font-size: 12px
      line-height: 24px
      width: 24px
      text-align: center
      color: rgb(147,153,159)
</style>
