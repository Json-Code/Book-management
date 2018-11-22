<template>
  <div class="comment-list">
    <div class="page-title" v-if="comments.length">
      我的评论
    </div>
    <div class="comment" v-for="(comment, index) in comments" :key="index">
      <div class="user">
        <div class="inline">
          <img :src="comment.image" class="avatar" mode="aspectFit" alt="">
          {{comment.title}}
        </div>
        <div class="right">
          {{comment.location || '未知地点'}}
          <span class="text-primary">
            --
          </span>
          {{comment.phone || '未知型号'}}
        </div>
      </div>
      <div class="content">
        {{comment.comment}}
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    props: ['comments']
  }
</script>

<style lang="stylus" scoped>
  .comment-list
    background: #eeeeee
    font-size: 14px
    .comment
      background: white
      margin-bottom: 10px
      padding: 5px 20px
      .user
        .inline
          display: inline
          .avatar
            vertical-align: middle
            width: 20px
            height: 20px
            border-radius: 50%
      .content
        margin: 10px 0

</style>