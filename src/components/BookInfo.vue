<template>
  <div class="bookinfo">
    <div class="thumb">
      <img class="back" :src="info.image" mode="aspectFill" alt="">
      <img class="img" :src="info.image" mode="aspectFit" alt="">
      <div class="info">
        <div class="title">
          {{info.title}}
        </div>
        <div class="author">
          {{info.author}}
        </div>
      </div>
    </div>
    <div class="detail">
      <img class="avatar" :src="userinfo.image" alt="" mode="aspectFit">
      {{userinfo.name}}
      <div class="right text-primary">
        {{info.rate}}分 <rate :value="info.rate"></rate>
      </div>
    </div>
    <div class="detail">
      {{info.publisher}}
      <div class="right">
        {{info.price}}
      </div>
    </div>
    <div class="tags">
      <div class="badge" v-for="(tag, index) in info.tags" :key="index">{{tag}}</div>
    </div>
    <div class="summary">
      <p v-for="(sum, index) in info.summary" :key="index">{{sum}}</p>
    </div>
  </div>
</template>

<script>
  import Rate from '@/components/Rate'
  export default {
    props: ['info'],
    computed: {
      userinfo () {
        return this.info.user_info || {}
      }
    },
    components: {
      Rate
    }
  }
</script>

<style lang="stylus" scoped>
  .bookinfo
    overflow: hidden
    .badge
      font-size: 12px
      display: inline-block
      margin: 5px
      padding: 5px
      border-radius: 10px
      border: 1px #EA5A49 solid
      color: #EA5A49
    .summary
      margin-top: 10px
      p
        padding: 0 15px
        text-indent: 2em
        font-size: 14px
    .right
      float: right
    .detail
      padding: 5px 10px
      .avatar
        width: 20px
        height: 20px
        border-radius: 50%
        vertical-align: middle
    .thumb
      position: relative
      width: 750rpx
      .back
        width: 100%
        filter: blur(15px)
      .img
        position: absolute
        width: 100%
        height: 300rpx
        left: 0
        top: 30rpx
      .info
        color: white
        position: absolute
        width: 100%
        left: 0
        top: 330rpx
        text-align: center
        .title
          font-size: 20px
        .author
          font-size: 14px
    
</style>