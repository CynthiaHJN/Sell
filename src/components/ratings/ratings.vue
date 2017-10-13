<template>
  <div class="ratings" ref="ratings">
    <div>
      <div class="seller-info">
        <div class="left">
          <div class="score">{{seller.score}}</div>
          <div class="title">综合评分</div>
          <div class="rank">高于周边商家{{seller.rankRate}}%</div>
        </div>
        <div class="right">
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
          <div class="score-wrapper">
            <span class="title">送达时间</span>
            <span class="delivery-time">{{seller.deliveryTime}}分钟</span>
          </div>
        </div>
      </div>
      <splite></splite>
      <ratingselect  @select="selectRating" @toggle="toggleContent" :select-type="selectType" :only-content="onlyContent" :desc="desc" :ratings="ratings"></ratingselect>
      <div class="rating-wrapper">
        <ul>
          <li class="rating-item border-1px" v-show="needShow(rating.rateType,rating.text)" v-for="rating in ratings">
            <div class="avatar">
              <img :src="rating.avatar" width="28" height="28">
            </div>
            <div class="content">
              <div class="name">{{rating.username}}</div>
              <div class="star-wrapper">
                <star :size="24" :score="rating.score"></star>
                <span class="delivery" v-show="rating.deliveryTime">{{rating.deliveryTime}}分钟送达</span>
              </div>
              <p class="text">{{rating.text}}</p>
              <div class="recommend">
                <span :class="{'icon-thumb_up':rating.rateType===0,'icon-thumb_down':rating.rateType===1}"></span>
                <span class="item" v-for="item in rating.recommend">{{item}}</span>
              </div>
              <div class="time">{{rating.rateTime}}</div>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll'
  import splite from '../../components/splite/splite.vue'
  import ratingselect from '../../components/ratingselect/ratingselect.vue'
  import star from '../../components/star/star.vue'
  import shopcart from '../../components/shopcart/shopcart.vue'
  const ERR_OK = 0
  const ALL = 2
  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data() {
      return {
        ratings: [],
        selectType: ALL,
        onlyContent: false,
        desc: {
          all: '全部',
          positive: '推荐',
          negative: '吐槽'
        }
      }
    },
    created() {
      this.$http.get('/api/ratings').then((response) => {
        response = response.body
        if (response.errno === ERR_OK) {
          this.ratings = response.data
          this.showFlag = true
          this.selectType = ALL
          this.onlyContent = false
          this.$nextTick(() => {
            this.scroll = new BScroll(this.$refs.ratings, {
              click: true
            })
          })
        }
      })
    },
    methods: {
      selectRating(type) {
        this.selectType = type
        this.$nextTick(() => {
          this.scroll.refresh()
        })
      },
      toggleContent() {
        this.onlyContent = !this.onlyContent
        this.$nextTick(() => {
          this.scroll.refresh()
        })
      },
      needShow(rateType, text) {
        if (this.onlyContent && !text) {
          return false
        }
        if (this.selectType === ALL) {
          return true
        } else {
          return rateType === this.selectType
        }
      }
    },
    components: {
      splite,
      ratingselect,
      star,
      shopcart
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl";
  .ratings
    position: absolute
    top: 174px
    bottom: 0
    left: 0
    width: 100%
    overflow: hidden
    .seller-info
      display: flex
      padding: 18px 0px
      .left
        flex: 0 0 137px
        text-align: center
        border-right: 1px solid rgba(147,153,159,.2)
        .score
          margin-bottom: 6px
          line-height: 28px
          font-size: 24px
          color: rgb(255,153,0)
        .title
          margin-bottom: 8px
          line-height: 12px
          font-size: 12px
          color: rgb(7,17,27)
        .rank
          line-height: 10px
          font-size: 10px
          color: rgb(147,153,159)
      .right
        flex: 1
        padding: 0 24px
        .score-wrapper
          margin-bottom: 8px
          font-size: 0
          &:last-child
            margin-bottom: 0
          .title
            line-height: 18px
            font-size: 12px
            color: rgb(7,17,27)
          .star
            display: inline-block
            vertical-align: top
            margin: 0 12px
          .score
            line-height: 18px
            font-size: 12px
            color: rgb(255,153,0)
          .delivery-time
            margin-left: 12px
            line-height: 18px
            font-size: 12px
            color: rgb(147,153,159)
    .rating-wrapper
      padding: 0 18px
      .rating-item
        display: flex
        padding: 18px 0
        border-1px(rgba(7,17,27,.1))
        .avatar
          flex: 0 0 28px
          img
            width: 28px
            height: 28px
            border-radius: 50%
        .content
          position: relative
          flex: 1
          margin-left: 12px
          .name
            margin-bottom: 4px
            line-height: 12px
            font-size: 12px
            color: rgb(7,17,27)
          .star-wrapper
            margin-bottom: 6px
            font-size:0
            .star
              display: inline-block
              vertical-align: top
            .delivery
              margin-left: 6px
              line-height: 12px
              font-size: 10px
              color: rgb(147,153,159)
          .text
            margin-bottom: 8px
            line-height: 18px
            font-size: 12px
            color: rgb(7,17,27)
          .recommend
            .icon-thumb_up,.icon-thumb_down
              line-height: 16px
              font-size: 12px
            .icon-thumb_up
              color: rgb(0,160,220)
            .icon-thumb_down
              color: rgb(147,153,159)
            .item
              margin: 0 8px
              padding: 0 6px
              line-height: 16px
              font-size: 12px
              color: rgb(147,153,159)
              border: 1px solid rgba(7,17,27,.1)
              border-radius: 2px
          .time
            position: absolute
            top: 0px
            right: 0px
            line-height: 12px
            font-size: 10px
            color: rgb(147,153,159)
</style>
