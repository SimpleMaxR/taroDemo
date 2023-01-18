<template>
  <!-- 注意，Vue 中点击事件需要绑定 @tap，-->
  <!-- 其余小程序事件名把 bind 换成 @ 即是 Taro 事件名（支付宝小程序除外，它的事件就是以 on 开头，需要把 on 换成 @）-->
  <view id="main-container">
    <view class="welcomeBanner">
      👏🏻 歡迎來自 {{welcomeWord}} 的用戶
    </view>
  </view>
</template>

<script>
import Vue from 'vue'
import Taro from '@tarojs/taro'
export default {
  methods: {
    handleClick(e) {
      console.log('handleClick')
      e.stopPropagation() // 阻止冒泡
    },
    handleScroll() {
      console.log('handleScroll')
    },
    handleScrollToUpper() {
      console.log('handleScrollToUpper')
    },
    navToItem1() {
      Taro.navigateTo({
        url: '/pages/item1/item1',
        success: function(res) {
          res.eventChannel.emit('acceptDataFromOpenerPage', { data: 'success navigate' })
        }
      })
    },
  },
  data() {
    return {
      wordToday: 'Hello World',
      welcomeWord: '',
      ipInfo: {},
    }
  },
  mounted() {
    const p = new Promise((resolve, reject) => {
      Taro.request({
        url: 'https://www.mxnzp.com/api/ip/self',
        data: {
          app_id:'rgihdrm0kslojqvm',
          app_secret:'WnhrK251TWlUUThqaVFWbG5OeGQwdz09'
        },
        success: function(res) {
          resolve(res.data)
        },
        fail: function(err) {
          reject(err)
        }
      })
    })
    p.then((res) => {
      this.ipInfo = res
      this.welcomeWord = (this.ipInfo['data'] instanceof Object) ? this.ipInfo['data']['desc'] : ''
    }, (err) => {
      console.log(err)
    })
  }
}
</script>

<style lang="scss">
#main-container {
  background-color: beige;
  color: #333;
  height: 100vh;
  width: 100vw;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
}

.welcomeBanner {
  font-size: 0.4rem;
  margin-top: 1rem;
  width: 80%;
  height: 1.5rem;
  line-height: 1.5rem;
  background-color: aliceblue;
  border: 3px solid #333;
  border-radius: 1rem;
  text-align: center;
}

</style>
