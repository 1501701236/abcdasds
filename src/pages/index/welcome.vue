<route lang="json5">
{
  style: {
    navigationBarTitleText: '账户登录',
  },
}
</route>
<template>
  <view class="bg-white overflow-hidden pt-2 px-4" :style="{ marginTop: safeAreaTop + 'px' }">
    <!-- 欢迎图片和内容 -->
    <image class="welcome-img" src="/static/images/welcome.png"></image>
    <view class="welcome-content">
      <text class="welcome-title">首次登录 领新人专享券包</text>
      <view class="welcome-btns">
        <button class="btn-primary" @tap="goToLogin()">手机号安全登录</button>
        <button class="btn-secondary" @tap="quickLogin()">一键登录</button>
      </view>
    </view>
  </view>
</template>

<script setup lang="ts">
// 解构 safeArea 的 top 值
const safeAreaTop = uni.getSystemInfoSync()

// 定义方法
function goToLogin() {
  uni.navigateTo({
    url: 'index',
  })
}

function quickLogin() {
  uni.showToast({
    title: '快速登录暂未实现',
    icon: 'none',
  })
}
</script>

<style>
.welcome {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
  margin: 0;
  background-color: #fff;
}

.welcome-img {
  width: 100%;
  height: 50%;
}
.welcome-content {
  text-align: center;
}
.welcome-title {
  font-size: 32rpx;
  font-weight: bold;
  color: #333;
  margin-bottom: 60rpx;
}
.welcome-btns {
  display: flex;
  flex-direction: column;
  gap: 20rpx;
}
.btn-primary {
  background-color: #5677fc;
  color: #fff;
  border-radius: 50rpx;
  height: 90rpx;
  line-height: 90rpx;
  text-align: center;
  font-size: 28rpx;
}
.btn-secondary {
  background-color: #4caf50;
  color: #fff;
  border-radius: 50rpx;
  height: 90rpx;
  line-height: 90rpx;
  text-align: center;
  font-size: 28rpx;
}
</style>
