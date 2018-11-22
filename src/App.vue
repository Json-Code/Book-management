<script>

export default {
  async created () {
    // 调用API从本地缓存中获取数据
    const logs = wx.getStorageSync('logs') || []
    logs.unshift(Date.now())
    wx.setStorageSync('logs', logs)

    console.log('app created and cache logs by setStorageSync')

    /*     // 设置登录地址已经废弃
    let user = wx.getStorageSync('userInfo')
    if (!user) {
      qcloud.setLoginUrl(config.loginUrl);
      qcloud.login({
          success: function (userInfo) {
              console.log('登录成功', userInfo)
              showSuccess('登录成功')
              wx.setStorageSync('userInfo', userInfo)
          },
          fail: function (err) {
              console.log('登录失败', err);
          }
      })
    } else {

    } */
  }
}
</script>

<style>
.containerimg {
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  padding: 200rpx 0;
  box-sizing: border-box;
}
/* this rule will be remove */
* {
  transition: width 2s;
  -moz-transition: width 2s;
  -webkit-transition: width 2s;
  -o-transition: width 2s;
}
/* 全局样式 */
.btn {
  color:  white;
  background: #EA5A49;
  margin-top: 10px;
  margin-bottom: 10px;
  padding-left: 15px;
  padding-right: 15px;
  border-radius: 2px;
  font-size: 16px;
  line-height: 40px;
  height: 40px;
  width: 100%;
}
.btn:active {
  background: #FA5A49;
}
.text-primary {
  color: #EA5A49
}
.text-footer {
  margin-top: 10px;
  text-align: center;
  font-size: 12px;
}
.right {
  float: right
}
.page-title {
  padding-left: 20px;
  line-height: 40px;
  font-size: 14px;
  background: #eee 
}
</style>
