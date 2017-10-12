<template>
  <transition name="move">
    <div class="food" v-show="showFlag">
      <div class="food-content">
        <div class="image-header">
          <img :src="food.image">
        </div>
      </div>
    </div>
  </transition>
</template>

<script type="text/ecmascript-6">
  export default {
    props: {
      food: {
        type: Object
      }
    },
    data() {
      return {
        showFlag: false
      }
    },
    methods: {
      show() {
        this.showFlag = true
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .food
    position: fixed
    top:0
    left: 0px
    bottom: 48px
    z-index: 30
    width: 100%
    background: #ffffff
    transform: translate3d(0,0,0)
    &.move-enter-active,&.move-leave-active
      transition: all .2s linear
    &.move-enter,&.move-leave-to
      transform: translate3d(100%,0,0)
</style>
