<template>
  <a :href="detailUrl">
    <div class="book-card">
      <div class="thumb" @click.stop="preview">
        <img :src="book.image" alt="" class="img" mode="aspectFit">
      </div>
      <div class="detail">
        <div class="row">
          <div class="right text-primary">
            {{book.rate}}<rate :value="book.rate"></rate>
          </div>
          <div class="left text-primary">
            {{book.title}}
          </div>
        </div> 
        <div class="row">
          <div class="right">
            浏览量：{{book.count}}   
          </div>
          <div class="left">
            {{book.author}}
          </div>
        </div>
        <div class="row">
          <div class="right">
            {{book.user_info.nickName}}
          </div>
          <div class="left">
            {{book.publisher}}
          </div>
        </div>            
      </div>
    </div>
  </a>  
</template>

<script>
  import Rate from '@/components/Rate'
  export default {
    props: ['book'],
    components: {
      Rate
    },
    computed: {
      detailUrl () {
        return '/pages/detail/main?id=' + this.book.id
      }
    },
    methods: {
      preview () {
        wx.previewImage({
          current: this.book.img,
          urls: [this.book.image]
        })
      }
    }
  }
</script>

<style lang="stylus" scoped>
  .book-card
    display: flex
    padding: 5px
    overflow hidden
    margin-top: 5px
    margin-bottom: 5px
    font-size: 14px
    .thumb
      width: 90px
      height: 90px
      margin: 0 auto
      overflow: hidden
      .img
        max-width: 100%
        max-height: 100%
    .detail
      flex: 1
      margin-right: 10px
      .row
        line-height: 20px
        margin-bottom: 3px
        width: 100%
        overflow: hidden
        .right
          float: right
        .left
          float: left
</style>