<template>
  <div id="app">
    <div class="container-wrapper" :class="{'blurs':isFilter}">
      <v-header :seller="seller" @click.native="showInfoAlert"></v-header>
      <div class="tab border-1px">
        <div class="tab-item">
          <router-link to="/goods">商品</router-link>
        </div>
        <div class="tab-item">
          <router-link to="/ratings">评论</router-link>
        </div>
        <div class="tab-item">
          <router-link to="/seller">商家</router-link>
        </div>
      </div>
      <router-view></router-view>
    </div>
    <div class="infoAlert" :class="{'disNone':isHide}">
      <v-sellerInfo :seller="seller"></v-sellerInfo>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import header from './components/header/header.vue'
  import sellerInfo from './components/sellerInfo/sellerInfo.vue'
  const ERR_OK = 0

  export default {
    data() {
      return {
        seller: {},
        isFilter: false,
        isHide: true
      }
    },
    created() {
      this.$http.get('/api/seller').then(response => {
        response = response.body
        if (response.errno === ERR_OK) {
          this.seller = response.data
          console.log(this.seller)
        }
      })
    },
    components: {
      'v-header': header,
      'v-sellerInfo': sellerInfo
    },
    methods: {
      showInfoAlert () {
        this.isFilter = true
        this.isHide = false
//        console.log(this.isFilter)
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  #app
    .blurs
      filter: blur(10px)
    .tab
      display:flex
      width:100%
      height: 40px
      line-height: 40px
      border-bottom: 1px solid rgba(7,17,27,.1)
      /*border-1px (rgba(7,17,27,.1))*/
      .tab-item
        flex: 1
        text-align: center
        & > a
          display: block
          font-size: 14px
          color: rgb(77, 85, 93)
          &.router-link-active
            color: rgb(240, 20, 20)
</style>
