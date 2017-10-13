<template>
  <div class="seller" ref="seller">
    <div class="seller-content">
      <div class="overview">
        <h1 class="title">{{seller.name}}</h1>
        <div class="desc border-1px">
          <star :size="36" :score="seller.score"></star>
          <span class="rating-count">({{seller.ratingCount}})</span>
          <span class="sell-count">月售{{seller.sellCount}}单</span>
        </div>
        <ul class="remark">
          <li class="block">
            <div class="title">起送价</div>
            <div class="price">{{seller.minPrice}}<span class="unit">元</span></div>
          </li>
          <li class="block">
            <div class="title">商家配送</div>
            <div class="price">{{seller.deliveryPrice}}<span class="unit">元</span></div>
          </li>
          <li class="block">
            <div class="title">平均配送时间</div>
            <div class="price">{{seller.deliveryTime}}<span class="unit">分钟</span></div>
          </li>
        </ul>
        <div class="favorite">
          <i class="icon-favorite" :class="{'collect':favorite,'no-collect':!favorite}"></i>
          <div class="text">{{favoriteText}}</div>
        </div>
      </div>
      <splite></splite>
      <div class="bulletin">
        <div class="title">公告与活动</div>
        <p class="text">{{seller.bulletin}}</p>
        <ul class="support">
          <li class="support-item" v-for="item in seller.supports">
            <span class="icon" :class="classMap[item.type]"></span>
            <span class="text">{{item.description}}</span>
          </li>
        </ul>
      </div>
      <splite></splite>
      <div class="pics">
        <h1 class="title">商家实景</h1>
        <div class="pic-wrapper" ref="picWrapper">
          <ul class="pic-list">
            <li class="pic-item" v-for="item in seller.pics">
              <img :src="item" height="90" width="120">
            </li>
          </ul>
        </div>
      </div>
      <splite></splite>
      <div class="info">
        <h1 class="title border-1px">商家信息</h1>
        <ul>
          <li class="info-item" v-for="info in seller.infos">{{info}}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll'
  import star from '../../components/star/star.vue'
  import splite from '../../components/splite/splite.vue'
  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data() {
      return {
        favorite: true
      }
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
    },
    watch: {
      'seller'() {
        this.$nextTick(() => {
          this._initScroll()
          this._initPics()
        })
      }
    },
    mounted() {
      this.$nextTick(() => {
        this._initScroll()
        this._initPics()
      })
    },
    computed: {
      favoriteText() {
        return this.favorite ? '已收藏' : '收藏'
      }
    },
    methods: {
      _initScroll() {
        if (!this.scroll) {
          this.scroll = new BScroll(this.$refs.seller, {
            click: true
          })
        } else {
          this.scroll.refresh()
        }
      },
      _initPics() {
        if (this.seller.pics) {
          let picWidth = 120
          let margin = 6
          let width = (picWidth + margin) * this.seller.pics.length - margin
          this.$refs.picWrapper.style.width = width + 'px'
          console.log(this.$refs.picWrapper.style.width)
          this.$nextTick(() => {
            if (!this.picScroll) {
              this.picScroll = new BScroll(this.$refs.picWrapper, {
                scrollX: true
              })
            } else {
              this.picScroll.refresh()
            }
          })
        }
      }
    },
    components: {
      star,
      splite
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl";
  .seller
    position: absolute
    top: 174px
    bottom: 0
    left: 0
    width: 100%
    overflow: hidden
    .overview
      position: relative
      padding: 18px
      .title
        margin-bottom: 8px
        line-height: 14px
        font-size: 14px
        color: rgb(7,17,27)
      .desc
        padding-bottom: 18px
        border-1px(rgba(7,17,27,.1))
        font-size: 0
        .star
          display: inline-block
          vertical-align: top
        .rating-count,.sell-count
          line-height: 18px
          font-size: 10px
          color: rgb(77,85,93)
        .rating-count
          margin: 0 12px 0 8px
      .remark
        display: flex
        .block
          flex: 1
          margin-top: 18px
          text-align: center
          border-right: 1px solid rgba(7,17,27,.1)
          &:last-child
            border-right: 0
          .title
            margin-bottom: 4px
            line-height: 10px
            font-size: 10px
            color: rgb(147,153,159)
          .price
            line-height: 24px
            font-size: 24px
            color: rgb(7,17,27)
            .unit
              font-size: 10px
      .favorite
        position: absolute
        top: 18px
        right: 18px
        width: 40px
        text-align: center
        .icon-favorite
          margin-bottom: 4px
          font-size: 24px
          line-height: 24px
          &.collect
            color: rgb(240,20,20)
          &.no-collect
            color: rgb(147,153,159)
        .text
          line-height: 10px
          font-size:10px
          colo: rgb(77,85,93)
    .bulletin
      padding: 18px 18px 0 18px
      .title
        margin-bottom: 8px
        line-height: 14px
        font-size: 14px
        color: rgb(7,17,27)
      p.text
        padding: 0 12px
        margin-bottom: 16px
        line-height: 24px
        font-size: 12px
        color: rgb(240,20,20)
      .support
        .support-item
          padding: 16px 12px
          border-top: 1px solid rgba(7,17,27,.1)
          .icon
            display: inline-block
            width: 16px
            height: 16px
            vertical-align: top
            margin-right: 6px
            background-size: 16px 16px
            background-repeat: no-repeat
            &.decrease
              bg-image ('decrease_4')
            &.discount
              bg-image ('discount_4')
            &.guarantee
              bg-image ('guarantee_4')
            &.invoice
              bg-image ('invoice_4')
            &.special
              bg-image ('special_4')
          .text
            line-height: 16px
            font-size: 12px
            color: rgb(7,17,27)
    .pics
      padding: 18px
      padding-right:0
      .title
        margin-bottom: 8px
        line-height: 14px
        font-size: 14px
        color: rgb(7,17,27)
      .pic-wrapper
        width: 100%
        overflow: hidden
        white-space: nowrap
        .pic-list
          font-size: 0
          .pic-item
            display: inline-block
            margin-right: 6px
            width: 120px
            height: 90px
            &:last-child
              margin: 0
    .info
      padding: 18px
      padding-bottom: 0
      .title
        margin-bottom: 8px
        line-height: 14px
        font-size: 14px
        color: rgb(7,17,27)
      .info-item
        padding: 16px 12px
        line-height: 16px
        font-size: 12px
        color: rgb(7,17,27)
        border-top: 1px solid rgba(7,17,27,.1)
</style>
