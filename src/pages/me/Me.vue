<template>
  <div class="containerimg">
    <div class="userinfo">
      <!-- 如果只是展示用户头像昵称，可以使用 <open-data /> 组件 -->
      <div class="avatar" v-if="userInfo">
        <open-data type="userAvatarUrl"></open-data>
      </div>
      <div v-if="userInfo">
        <open-data type="userNickName"></open-data>
      </div>
      <!-- 需要使用 button 来授权登录 -->
      <button v-if="!userInfo" open-type="getUserInfo" @getuserinfo="bindGetUserInfo">授权登录</button>
    </div>
    <year-progress></year-progress>
    <button class="btn" @click="scanBook">添加图书</button>
  </div>
</template>

<script>
  import {showSuccess, showModal} from '../../utils/index.js'
  import {post} from '../../utils/index.js'
  import YearProgress from '@/components/YearProgress'

  export default {
    components: {
      YearProgress
    },
    data () {
      return {
        canIUse: wx.canIUse('button.open-type.getUserInfo'),
        userInfo: null,
        openId: null
      }
    },
    methods: {
      bindGetUserInfo (e) {
        const userInfo = e.mp.detail.userInfo
        wx.login({
          success (res) {
            if (res.code) {
              // 发起网络请求
              wx.request({
                url: `https://api.weixin.qq.com/sns/jscode2session?appid=wx44772d6b4c4b8ef0&secret=9dab9e3037562ba487d2be9b0113ebdc&js_code=${res.code}&grant_type=authorization_code`,
                data: {
                  code: res.code
                },
                success: function (res) {
                  wx.setStorageSync('openId', res.data)
                }
              })
            } else {
              console.log('登录失败！' + res.errMsg)
            }
          }
        })
        // const openId = wx.getStorageSync('openid')
        // wx.setStorageSync('userInfo', {...userInfo, ...openId})
        wx.setStorageSync('userInfo', userInfo)
        showSuccess('登录成功')
        this.userInfo = wx.getStorageSync('userInfo')
      },
      scanBook () {
        // 允许从相机和相册扫码
        wx.scanCode({
          success: (res) => {
            if (res.result) {
              this.addBook(res.result)
            }
          }
        })
      },
      async addBook (isbn) {
        const res = await post('/weapp/addbook', {
          isbn,
          openid: wx.getStorageSync('openId').openid
        })
        showModal('添加成功', `${res.title}添加成功`)
      }
    },
    created () {
      // 查看是否授权
      wx.getSetting({
        success (res) {
          if (res.authSetting['scope.userInfo']) {
            // 已经授权，可以直接调用 getUserInfo 获取头像昵称
            wx.getUserInfo({
              success: function (res) {
                // 获取缓存
              }
            })
          }
        }
      })
      this.userInfo = wx.getStorageSync('userInfo')
      this.openId = wx.getStorageSync('openId')
    }
  }
</script>

<style lang="stylus" scoped>
  .containerimg
    padding: 0 30rpx
    .userinfo
      margin-top: 100rpx
      text-align: center
      .avatar
        width: 200rpx
        height: 200rpx
        border-radius: 50%
        overflow: hidden
</style>

