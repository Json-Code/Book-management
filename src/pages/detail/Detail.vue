<template>
  <div>
    <book-info :info="info"></book-info>
    <comment-list :comments="comments"></comment-list>
    <div class="comment" v-if="showAdd">
      <textarea v-model="comment" class="textarea" :maxlength="100" placeholder="请输入图书评论"/>
      <div class="location">
        地理位置:
        <switch color="#EA5A49" :checked="location" @change="getGeo"></switch>
        <span class="text-primary">{{location}}</span>
      </div>
      <div class="phone">
        手机型号:
        <switch color="#EA5A49" :checked="phone" @change="getPhone"></switch>
        <span class="text-primary">{{phone}}</span>
      </div>
      <button class="btn" @click='addComment'>
        评论
      </button>
    </div>
    <div v-else class="text-footer">
      未登录或者已经评论过
    </div>
    <button class="btn" open-type="share">转发给好友</button>
  </div>
</template>

<script>
  import {get, showModal, post} from '@/utils'
  import BookInfo from '@/components/BookInfo'
  import CommentList from '@/components/CommentList'
  export default {
    data () {
      return {
        comments: [],
        userinfo: {},
        bookid: '',
        info: {},
        comment: '',
        location: '',
        phone: ''
      }
    },
    computed: {
      showAdd () {
        // 没登录
        if (!this.userinfo.openid) {
          return false
        }
        // 评论列表查到自己的评论
        if (this.comments.filter(v => v.openid === this.userinfo.openid).length) {
          return false
        }
        return true
      }
    },
    onShareAppMessage (res) {
      if (res.from === 'button') {
        // 来自页面内转发按钮
        console.log(res.target)
      }
    },
    methods: {
      async addComment () {
        if (!this.comment) {
          return
        }
        // 评论内容 手机型号  地理位置 图书id 用户的openid
        const data = {
          openid: this.userinfo.openid,
          bookid: this.bookid,
          comment: this.comment,
          phone: this.phone,
          location: this.location
        }
        try {
          await post('/weapp/addcomment', data)
          this.comment = ''
          this.getComments()
        } catch (e) {
          console.log('err')
          showModal('失败', e.msg)
        }
      },
      async getDetail () {
        const info = await get('/weapp/bookdetail', {id: this.bookid})
        wx.setNavigationBarTitle({
          title: info.title
        })
        this.info = info
      },
      async getComments () {
        const comments = await get('/weapp/commentlist', {bookid: this.bookid})
        console.log('comments', comments)
        this.comments = comments.list || []
      },
      getGeo (e) {
        const ak = 'USmciC0n8aXUhWfYLAsvXuyEaMVQNqdt'
        let url = 'http://api.map.baidu.com/geocoder/v2/'
        if (e.target.value) {
          wx.getLocation({
            success: geo => {
              wx.request({
                url,
                data: {
                  ak,
                  location: `${geo.latitude},${geo.longitude}`,
                  output: 'json'
                },
                success: res => {
                  console.log(res)
                  if (res.data.status === 0) {
                    this.location = res.data.result.addressComponent.city
                  } else {
                    this.location = '未知地点'
                    // console.log('出错了')
                  }
                }
              })
            }
          })
        } else {
          this.location = ''
        }
      },
      getPhone (e) {
        if (e.target.value) {
          const phoneInfo = wx.getSystemInfoSync()
          this.phone = phoneInfo.model
        } else {
          this.phone = ''
        }
      }
    },
    mounted () {
      this.bookid = this.$root.$mp.query.id
      this.getDetail()
      this.getComments()
      const userinfo = wx.getStorageSync('openId')
      if (userinfo) {
        this.userinfo = userinfo
      }
    },
    components: {
      BookInfo,
      CommentList
    }
  }
</script>

<style lang="stylus" scoped>
  .comment
    margin-top: 10px
    .btn
      margin-top: 5px
    .textarea
      width: 730rpx
      height: 200rpx
      background: #eee
      padding: 10rpx
    .location
      margin-top: 10px
    .phone
      margin-top: 10px
</style>