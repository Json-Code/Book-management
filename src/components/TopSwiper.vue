<template>
  <div class="swiper">
    <swiper
          :indicator-dots = 'true'
          indicator-color = '#EA5A49'
          :autoplay = 'true'
          :interval = '6000'
          :duration = '1000'
          :circular = 'true'
    >
      <div v-for="(top, index) in imgUrls" :key="index">
        <swiper-item>
          <img @click="bookDetail(img)" v-for="(img, indexs) in top" :key="indexs"  class="slider-image" mode="aspectFit" :src="img.image" alt="">
        </swiper-item>
      </div>
    </swiper>
  </div>
</template>

<script>
  export default {
    props: ['tops'],
    computed: {
      imgUrls () {
        // 如果通用 请用chunk函数 比如lodash的chunkfang方法
        let res = this.tops
        return [res.slice(0, 3), res.slice(3, 6), res.slice(6)]
      }
    },
    methods: {
      bookDetail (item) {
        wx.navigateTo({
          url: '/pages/detail/main?id=' + item.id
        })
      }
    }
  }
</script>

<style lang="stylus" scoped>
  .swiper
    margin-top: 5px
    .slider-image
      width: 33%
      height: 250rpx
</style>