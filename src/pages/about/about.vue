<route lang="json5">
{
  style: {
    navigationStyle: 'custom',
  },
}
</route>
<template>
  <view>
    <!-- Background Image -->
    <view
      class="relative w-full h-58 bg-cover bg-center flex items-center"
      :style="'background-image: url(' + pic[topBackGroupImageIndex].link + ')'"
    >
      <!-- User Avatar and Name -->
      <view class="flex items-center space-x-3 px-6" @click="goMedal">
        <view class="w-16 h-16 rounded-full overflow-hidden shadow-md bg-white">
          <image src="../../static/logo.png" class="w-full h-full object-cover" />
        </view>
        <view class="text-white text-lg font-bold">{{ userName }}</view>
        <uni-icons type="right" size="24" class="center"></uni-icons>
      </view>
    </view>

    <!-- Level Information -->
    <view class="relative -mt-18 bg-white px-6 py-4 rounded-t-xl shadow-lg mx-4">
      <!-- Expiry Date Container -->
      <view
        class="absolute top-0 right-0 bg-black text-white text-xs px-3 py-1 rounded-bl-lg shadow-sm"
      >
        {{ expiryDate }}
      </view>
      <view class="flex justify-between items-center">
        <!-- Left Level Info -->
        <view>
          <view class="text-lg font-bold text-yellow-500">{{ levelTitle }}</view>
          <view class="text-gray-500 text-sm">{{ expiryDate }}</view>
        </view>
        <!-- Progress Bar -->
        <view class="flex-1 mx-4">
          <view class="h-2 rounded-full bg-gray-200 overflow-hidden">
            <view
              class="h-full bg-yellow-400 rounded-full"
              :style="'width: ' + progressWidth + '%'"
            />
          </view>
        </view>
        <!-- Current Score -->
        <view class="text-lg font-bold text-gray-800">{{ currentScore }}</view>
      </view>
    </view>

    <!-- 菜单容器 -->
    <view class="mt-5 bg-gray-100 rounded-lg p-4 px-6">
      <!-- Grid Menu -->
      <uni-grid :column="5" :highlight="true" :showBorder="false" :square="false">
        <uni-grid-item v-for="(item, index) in displayedMenuItems" :key="index" :index="index">
          <view class="flex flex-col items-center justify-center h-full py-4">
            <uni-icons :type="item.icon" :size="30" class="text-gray-500" />
            <text class="mt-2 text-sm text-gray-800">{{ item.name }}</text>
          </view>
        </uni-grid-item>
      </uni-grid>

      <!-- Toggle Button -->
      <view class="flex justify-center">
        <uni-icons
          class="center"
          :type="isExpanded ? 'up' : 'down'"
          :size="30"
          @click="toggleExpand"
        ></uni-icons>
      </view>
    </view>
  </view>

  // 生成一个显示勋章的区域
  <view class="relative w-full h-30">
    <!-- 轮播图 -->
    <swiper
      autoplay="true"
      :interval="3000"
      :duration="500"
      :circular="true"
      :indicator-dots="true"
      :current="current"
      @change="onSwiperChange"
      class="w-full h-full"
    >
      <!-- 轮播项 -->
      <swiper-item
        v-for="(item, index) in images"
        :key="index"
        class="flex flex-col items-center justify-center h-full"
      >
        <image class="w-full" :src="item" />
      </swiper-item>
    </swiper>
    <!-- 当前索引 -->
    <view
      class="absolute bottom-2 right-2 bg-black bg-opacity-50 text-white text-xs px-3 py-1 rounded-full"
    >
      {{ current + 1 }}/{{ images.length }}
    </view>
  </view>

  <!-- Calendar Grid -->
  <view class="p-4 bg-white rounded-md shadow-sm">
    <!-- Header -->
    <view class="flex justify-between items-center mb-4">
      <text class="text-lg font-bold">{{ currentMonth }}月咖啡日历</text>
      <text class="text-sm text-blue-500 cursor-pointer" @click="goToLastMonth">
        查看12月咖啡月历
      </text>
    </view>

    <!-- Calendar Grid -->
    <view class="grid grid-cols-7 gap-2 text-center">
      <!-- Weekday Labels -->
      <view class="text-gray-500 font-bold" v-for="day in weekdays" :key="day">{{ day }}</view>

      <!-- Date Items -->
      <view
        v-for="(day, index) in days"
        :key="index"
        class="h-12 flex items-center justify-center cursor-pointer"
        :class="day.isSigned ? 'bg-yellow-500 text-white rounded-full' : 'text-gray-800'"
        @click="onDayClick(day)"
      >
        {{ day.date }}
      </view>
    </view>
  </view>
</template>

<script lang="ts" setup>
const weekdays = ref(['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'])
const currentMonth = ref(new Date().getMonth() + 1) // 当前月份
const days = ref([])

// 加载当月的天数和签到状态
const loadDays = () => {
  const date = new Date()
  const year = date.getFullYear()
  const month = currentMonth.value

  // 当月天数
  const daysInMonth = new Date(year, month, 0).getDate()

  // 动态生成日期
  days.value = Array.from({ length: daysInMonth }, (_, i) => ({
    date: i + 1, // 日期
    isSigned: Math.random() > 0.7, // 随机签到状态，实际可替换为后端返回的数据
  }))
}

// 签到点击事件
const onDayClick = (day) => {
  if (!day.isSigned) {
    day.isSigned = true // 模拟签到
    console.log(`签到成功：${day.date}`)
  }
}

// 切换至12月
const goToLastMonth = () => {
  currentMonth.value = 12
  loadDays()
}

const userName = ref('迪迦奥特曼')
const expiryDate = ref('2025-03-06 到期')
const levelTitle = ref('Lv3 小金鹿')
const progressWidth = ref(70)
const currentScore = ref(1101)
const isExpanded = ref(false)
// 图片列表
const images = [
  'https://picsum.photos/id/210/200/300',
  'https://picsum.photos/id/220/200/300',
  'https://picsum.photos/id/237/200/300',
  'https://picsum.photos/id/250/200/300?',
]
// 当前轮播图索引
const current = ref(0)

// 监听轮播图切换
const onSwiperChange = (e) => {
  current.value = e.detail.current
}
const menuItems = [
  { name: '我的订单', icon: 'list' },
  { name: '咖啡钱包', icon: 'wallet' },
  { name: '优惠券', icon: 'wallet' },
  { name: '招商加盟', icon: 'wallet' },
  { name: '账户余额', icon: 'wallet' },
  { name: '发票管理', icon: 'wallet' },
  { name: '兑换优惠', icon: 'gift' },
  { name: '帮助反馈', icon: 'help' },
  { name: '礼品卡', icon: 'wallet' },
  { name: '口味定制', icon: 'wallet' },
  { name: '活动周边', icon: 'wallet' },
  { name: '多人团餐', icon: 'wallet' },
  { name: '关注微信', icon: 'wallet' },
]

const displayedMenuItems = computed(() => {
  return isExpanded.value ? menuItems : menuItems.slice(0, 5)
})

const toggleExpand = () => {
  isExpanded.value = !isExpanded.value
}
const goMedal = () => {
  uni.navigateTo({
    url: 'medal',
  })
}
const pic = [
  { link: 'https://cdn.zhoukaiwen.com/zjx_me_bg1.jpeg', name: '春天' },
  { link: 'https://cdn.zhoukaiwen.com/zjx_me_bg2.jpeg', name: '夏天' },
  { link: 'https://cdn.zhoukaiwen.com/zjx_me_bg3.jpeg', name: '秋天' },
  { link: 'https://cdn.zhoukaiwen.com/zjx_me_bg4.jpeg', name: '冬天' },
  { link: 'https://cdn.zhoukaiwen.com/zjx_me_bg5.jpeg', name: '幽静' },
  { link: 'https://cdn.zhoukaiwen.com/zjx_me_bg6.jpg', name: '天空' },
]
// 初始化加载
loadDays()
const topBackGroupImageIndex = ref(5)
</script>

<style scoped>
.relative {
  position: relative;
}

.bg-cover {
  background-size: cover;
}

.bg-center {
  background-position: center;
}

.rounded-full {
  border-radius: 9999px;
}

.shadow-md {
  box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
}

.flex {
  display: flex;
}

.items-center {
  align-items: center;
}

.justify-between {
  justify-content: space-between;
}

.space-x-3 > view {
  margin-left: 12px;
}

.px-6 {
  padding-left: 24px;
  padding-right: 24px;
}

.text-lg {
  font-size: 1.125rem;
  line-height: 1.75rem;
}

.font-bold {
  font-weight: 700;
}

.text-yellow-500 {
  color: #f59e0b;
}

.text-gray-500 {
  color: #6b7280;
}

.text-gray-800 {
  color: #1f2937;
}

.text-white {
  color: white;
}

.bg-white {
  background-color: white;
}
</style>
