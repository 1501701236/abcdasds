<!-- 使用 type="home" 属性设置首页，其他页面不需要设置，默认为page；推荐使用json5，更强大，且允许注释 -->
<route lang="json5" type="home">
{
  style: {
    navigationBarTitleText: '首页',
  },
}
</route>
<template>
  <!-- 灰色背景 -->
  <view class="bg-[#f5f5f5] w-full min-h-[100vh] flex justify-center">
    <!-- 白色内容区域 -->
    <view
      class="bg-[#ffffff] p-[20rpx] mt-50px rounded-[20rpx] shadow-md w-full max-w-[700rpx] h-130 box-border flex-none"
    >
      <!-- 顶部图片 -->
      <image
        class="w-full h-[40vh] mb-[30rpx] rounded-[20rpx]"
        src="/static/images/welcome.png"
      ></image>
      <!-- 标题文字 -->
      <view class="text-center text-[32rpx] font-bold text-[#333] mb-[40rpx]">
        首次登录 领新人专享券包
      </view>
      <!-- 登录按钮区域 -->
      <view class="flex flex-col gap-[20rpx]">
        <button
          class="w-full bg-[#5677fc] text-white rounded-[50rpx] text-[28rpx] text-center"
          @tap="goToLogin"
        >
          手机号安全登录
        </button>
        <button
          class="w-full bg-[#4caf50] text-white rounded-[50rpx] text-[28rpx] text-center"
          @tap="quickLogin"
        >
          一键登录
        </button>
      </view>

      <!-- 协议文字 -->
      <view class="text-[24rpx] text-[#888] mt-[40rpx] text-center">
        <checkbox value="cb" checked="true"  />
        我已阅读并同意
        <text class="text-[#5677fc]">《瑞幸咖啡用户协议》</text>
        <text class="text-[#5677fc]">《瑞幸咖啡隐私政策》</text>
      </view>
    </view>
  </view>
</template>

<script lang="ts" setup>
import { TestEnum } from '@/typings'

defineOptions({
  name: 'Home',
})
// 定义方法
function goToLogin() {
  uni.navigateTo({
    url: 'index1',
  })
}

function quickLogin() {
  uni.showToast({
    title: '快速登录暂未实现',
    icon: 'none',
  })
}
function handleChange({ value }) {
  console.log(value)
}
// 获取屏幕边界到安全区域距离
const { safeAreaInsets } = uni.getSystemInfoSync()
const value = ref<boolean>(true)
const author = ref('菲鸽')
// 测试 uni API 自动引入
onLoad(() => {
  console.log(author)
  console.log(TestEnum.A)
})
</script>

<style>
</style>
