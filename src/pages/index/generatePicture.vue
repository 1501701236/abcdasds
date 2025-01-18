<route lang="json5" type="home">
{
  style: {
    navigationBarTitleText: '生成美图',
  },
}
</route>
<template>
  <view class="p-4 bg-white">
    <!-- 返回按钮 -->
    <view class="flex items-center mb-4 cursor-pointer" @click="goBack">
      <uni-icons type="left" size="20" class="text-gray-500"></uni-icons>
      <span class="ml-2 text-gray-500">返回</span>
    </view>

    <!-- 日期部分 -->
    <view class="text-center">
      <span class="block text-2xl font-bold text-gray-800">{{ month }}</span>
      <span class="block text-7xl font-bold text-gray-800">{{ day }}</span>
    </view>

    <!-- 标题 -->
    <view class="text-center mt-4">
      <span class="text-lg font-bold">{{ title }}</span>
    </view>

    <!-- 图片 -->
    <view class="mt-6 flex justify-center">
      <img :src="imageSrc" alt="美图" class="w-full max-w-300px" />
    </view>

    <!-- 名言部分 -->
    <view class="text-center mt-6">
      <view class="block text-gray-600 italic">{{ quote }}</view>
      <view class="block text-sm text-gray-500 mt-2">{{ author }}</view>
    </view>

    <!-- 操作按钮 -->
    <view class="flex justify-center mt-10 space-x-4">
      <button class="bg-yellow-500 text-white px-6 py-2 rounded-md" @click="shareImage">
        分享
      </button>
      <button class="bg-blue-500 text-white px-6 py-2 rounded-md" @click="saveImage">保存</button>
    </view>
  </view>
</template>

<script setup>
const date = ref('') // 路由参数日期
const day = ref('') // 日
const month = ref('') // 月
const title = ref('宜活在此刻') // 默认标题
const quote = ref('不惋惜，不思欲，现在世界就是我的。') // 名言
const author = ref('《别想太多啦》 作家：名取芳彦') // 作者
const imageSrc = ref('https://example.com/default-image.jpg') // 默认图片地址

// 返回上一页
const goBack = () => {
  router.back()
}

// 格式化月份
const formatMonth = (month) => {
  const months = [
    'January',
    'February',
    'March',
    'April',
    'May',
    'June',
    'July',
    'August',
    'September',
    'October',
    'November',
    'December',
  ]
  return months[parseInt(month, 10) - 1] || ''
}

// 保存图片到相册
const saveImage = () => {
  uni.showLoading({ title: '下载中...', mask: true }) // 显示加载动画

  uni.downloadFile({
    url: imageSrc.value,
    success: (res) => {
      if (res.tempFilePath) {
        uni.saveImageToPhotosAlbum({
          filePath: res.tempFilePath,
          success: () => {
            uni.hideLoading() // 隐藏加载动画
            uni.showToast({ title: '保存成功', icon: 'success' })
          },
          fail: (err) => {
            uni.hideLoading() // 隐藏加载动画
            uni.showToast({ title: '保存失败', icon: 'none' })
            console.error('保存失败:', err)
          },
        })
      }
    },
    fail: (err) => {
      uni.hideLoading() // 隐藏加载动画
      uni.showToast({ title: '下载失败', icon: 'none' })
      console.error('下载失败:', err)
    },
  })
}

// 分享功能（扩展用）
const shareImage = () => {
  uni.showToast({ title: '分享功能未实现', icon: 'none' })
}

// 路由加载参数
onMounted(() => {
  // date.value = route.query.date || ''
  // const [year, monthStr, dayStr] = date.value.split('-')
  // month.value = formatMonth(monthStr)
  // day.value = dayStr
  //
  // // 根据日期动态设置内容
  // if (date.value === '2025-01-02') {
  imageSrc.value = 'https://example.com/special-image.jpg'
  quote.value = '生活的每一刻都值得欣赏。'
  author.value = '《特别的你》 作家：某某某'
  // }
})
</script>

<style scoped>
/* 无需自定义样式，全部通过 UnoCSS 的原子化类实现 */
</style>
