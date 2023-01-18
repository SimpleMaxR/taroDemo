<template>
  <view class="text">{{msg}}</view>
</template>

<script>
import Taro from '@tarojs/taro'
export default {
  data() {
    return {
      msg: 'Welcome to item1'
    }
  },
  mounted(res) {
    Taro.Events.on('acceptDataFromOpenerPage', this, function(data) {
      Taro.showToast({
        title: data.data,
        icon: 'success',
      })
    })

  }
}
</script>

<style lang="scss">
.text{
  height: 100vh;
  width: 100vw;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 1rem;
  background-color: beige;
  color: #333;
}
</style>
