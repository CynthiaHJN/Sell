<template>
  <div class="infoContent">
    <div class="total">
      <div class="title">{{seller.name}}</div>
      <div class="stars"></div>
    </div>
    <div class="supportsInfo">
      <div class="title parting-line">优惠信息</div>
      <div class="content" v-for="item in seller.supports">
        <span class="icon" :class="classMap[item.type]"></span>
        <span class="text">{{item.description}}</span>
      </div>
    </div>
    <div class="bulletin">
      <div class="title parting-line">商家公告</div>
      <div class="content">{{seller.bulletin}}</div>
    </div>
    <div style="text-align: center;margin-top: 20%;">
      <a href="javascript:;" class="close-btn"><i class="icon icon-close"></i></a>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {
    props: {
      seller: {
        type: Object
      }
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl";
  .infoContent
    position: fixed
    left: 0
    right: 0
    top: 0
    bottom: 0
    background-color: rgba(7,17,27,.8)
    z-index: 100
    color: #fff
    .total
      .title
        margin-top: 15%
        line-height: 16px
        font-size: 16px
        font-weight: 700
        color: #fff
        text-align: center
    .supportsInfo
      margin: 0 36px
      .title
        margin-bottom: 12px
      .content
        padding: 12px 12px 0 12px
        font-size: 12px
        line-height: 12px
        .icon
          display: inline-block
          vertical-align: top
          width: 12px
          height: 12px
          margin-right: 4px
          background-size: 12px 12px
          background-repeat: no-repeat
          &.decrease
            bg-image ('decrease_1')
          &.discount
            bg-image ('discount_1')
          &.guarantee
            bg-image ('guarantee_1')
          &.invoice
            bg-image ('invoice_1')
          &.special
            bg-image ('special_1')
    .bulletin
      margin: 0 36px
      .content
        padding: 24px 12px 0 12px
        font-size: 12px
        line-height: 24px
    .close-btn
      display: inline-block
      margin: 20px auto 32px auto
      .icon
        font-size: 32px
        color: rgba(255,255,255,.5)
        cursor: pointer
</style>
