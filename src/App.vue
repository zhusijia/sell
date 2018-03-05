<template>
  <div id="app">
    <v-header></v-header>
    <div class="tab border-1px">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
        <!-- <a v-link="{path: '/goods'}">商品</a> -->
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
        <!-- <a v-link="{path: '/ratings'}">评论</a> -->
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
        <!-- <a v-link="{path: '/seller'}">商家</a> -->
      </div>
    </div>
    <router-view></router-view>
  </div>
</template>

<script>
  import header from './components/header/header.vue';
  const data = {
    msg: "Hello Vue!"
  }
  export default {
    name: 'App',
    components: {
      "v-header": header
    },
    data () {
      return data;
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "./common/stylus/mixin.styl";
  #app
    .tab
      display flex
      width 100%
      height 40px
      line-height 40px
      border-1px(rgba(7, 17, 271 0.1))
      .tab-item
        flex 1
        text-align center
        & > a
          display block
          font-size 14px
          color rgb(77, 85, 93)
          &.active
            color rgb(240, 20, 20)
</style>
