<script setup lang="ts">
// 时间选择
const props = withDefaults(defineProps<{
  defaultTime?: number // 默认日期
  mode?: 'single' | 'range' | 'week' | 'show' // 选择模式
  value: number | [number, number] // 毫秒时间戳 一个时间戳表示选中的日期，或者一个时间戳数组表示选中的日期区间
  startWeek?: 0 | 1 // 一周的开始是周几
}>(), {
  mode: 'show',
  defaultTime: new Date().getTime(),
  startWeek: 0,
})
/*

function to(path: string) {
  uni.navigateTo({
    url: path,
  })
}

const userStore = useUserStore()

function handleLogout() {
  userStore.logout().then(() => {
    // 重定向到登录页面
    uni.redirectTo({
      url: '/pages/login/index',
    })
  })
}
*/

const now = props.defaultTime ? new Date(props.defaultTime) : new Date()
const year = ref(now.getFullYear())
const month = ref(now.getMonth() + 1)
// 获取YYYY-MM
const date = computed(() => `${year.value}-${month.value.toString().padStart(2, '0')}`)

function changeDate(v: string) {
  const [y, m] = v.split('-')
  year.value = Number(y)
  month.value = Number(m)
}

function handleChangeLastMonth() {
  if (month.value === 1) {
    year.value -= 1
    month.value = 12
  }
  else {
    month.value -= 1
  }
}

function handleChangeNextMonth() {
  if (month.value === 12) {
    year.value += 1
    month.value = 1
  }
  else {
    month.value += 1
  }
}

const dates = ref([
  { date: '1', homework: '4' },
  { date: '2', homework: '' },
  { date: '3', homework: '' },
  { date: '4', homework: '5' },
  { date: '5', homework: '' },
  { date: '6', homework: '' },
  { date: '7', homework: '' },
  { date: '8', homework: '' },
  { date: '9', homework: '4' },
  { date: '10', homework: '' },
  { date: '11', homework: '' },
  { date: '12', homework: '' },
  { date: '13', homework: '5' },
])
const selectedIndex = ref(0) // 用于记录当前选中的项

function selectItem(index) {
  selectedIndex.value = index // 更新选中的项
}

const drawerVisible = ref(false)
const maskClick = ref(true)
const mode = ref<'bottom' | 'top' | 'left' | 'right'>('bottom')
const showRadius = ref(true)

// 触发评价
function assess() {
  drawerVisible.value = true
}

function onDrawerClosed() {
  drawerVisible.value = false
  uni.showToast({
    title: '关闭',
    icon: 'none',
  })
}

const rating = ref(0)
const stars = ref([1, 2, 3, 4, 5])

function setRating(value) {
  rating.value = value
}
</script>

<template>
  <view style="background-color: #f6f5fa; min-height: 100vh">
    <view class="top-bg p-3">
      <view class="mt-60rpx text-xl font-bold">
        作业列表
      </view>
      <!-- 头部 -->
      <view class="flex-center leading-80rpx">
        <view class="i-carbon:chevron-left px-5" @tap="handleChangeLastMonth" />
        <picker mode="date" fields="month" :value="date" @change="(e: any) => changeDate(e.detail.value)">
          {{ date }}
        </picker>
        <view class="i-carbon:chevron-right px-5" @tap="handleChangeNextMonth" />
      </view>
      <view class="container">
        <scroll-view :show-scrollbar="false" scroll-x="true" class="scroll-view">
          <view style="display: flex;">
            <view
              v-for="(item, index) in dates" :key="index" class="date-item"
              :class="{
                selected: selectedIndex === index && item.homework,
                noWork: selectedIndex === index && !item.homework,
              }"
              @click="selectItem(index)"
            >
              <text class="date">
                {{ item.date }}
              </text>
              <view>
                <text v-if="index !== selectedIndex && item.homework" class="point">
                  .
                </text>
                <view class="homework">
                  <view v-if="index === selectedIndex && item.homework">
                    {{ item.homework }}份作业
                  </view>
                </view>
              </view>
            </view>
          </view>
        </scroll-view>
      </view>
      <view v-if="dates[selectedIndex].homework" class="list">
        <view>
          <view class="mt-60rpx text-xl font-bold">
            <text style="font-size: 30px">
              {{ dates[selectedIndex].homework }}
            </text>
            份作业
          </view>
          <!--      单元格头 -->
          <text class="all">
            全部学科 >
          </text>
        </view>
        <!--     默认显示四个 语数外历史 -->
        <view class="subjects">
          <view class="subjects-item">
            <view class="subjectsName">
              icon--语文
            </view>
            <view class="num">
              2
            </view>
          </view>
          <view class="subjects-item">
            <view class="subjectsName">
              icon--英语
            </view>
            <view class="num">
              1
            </view>
          </view>
          <view class="subjects-item">
            <view class="subjectsName">
              icon--数学
            </view>
            <view class="num">
              1
            </view>
          </view>
          <view class="subjects-item">
            <view class="subjectsName">
              icon--历史
            </view>
            <view class="num">
              1
            </view>
          </view>
        </view>
        <view class="card">
          <!--        分类 题目 状态 -->
          <view class="card-title">
            <view class="left">
              <view class="classify">
                <text>语文基础</text>
              </view>
              <view style="margin-left: 20rpx">
                关于孔乙己的理解
              </view>
            </view>
            <view v-if="true" class="right">
              icon 已批改
            </view>
            <view v-else class="right">
              icon 未批改
            </view>
          </view>
          <!--          剩余时间 -->
          <text style="color:#949698; font-size: 12px">
            icon-12天后作业结束
          </text>
          <!--          作业详情和操作按钮 -->
          <view style="width: 100%;display: flex; margin-top: 20px;justify-content: space-between">
            <view>
              <view>
                <text style="font-size: 18px;color: red">
                  5
                </text>
                /20
              </view>
              <view class="info">
                错题数/总数
              </view>
            </view>
            <view v-if="true" style="margin-left: 30rpx">
              <view style="color: #1ab270">
                <text style="font-size: 18px">
                  78
                </text>
                %
              </view>
              <view class="info">
                正确率
              </view>
            </view>
            <view class="btn-2">
              <button
                type="default" size="mini" plain style="border-radius: 20px;height: 35px;line-height: 35px"
                @click="assess"
              >
                评价
              </button>
              <button
                type="primary" size="mini"
                style="border-radius: 20px;height: 35px;line-height: 35px;margin-left: 8px;"
              >
                学情报告
              </button>
            </view>
          </view>
        </view>
        <view class="card">
          <!--        分类 题目 状态 -->
          <view class="card-title">
            <view class="left">
              <view class="classify">
                <text>语文基础</text>
              </view>
              <view style="margin-left: 20rpx">
                关于孔乙己的理解
              </view>
            </view>
            <view v-if="false" class="right">
              icon 已批改
            </view>
            <view v-else class="right">
              icon 未批改
            </view>
          </view>
          <!--          剩余时间 -->
          <text style="color:#949698; font-size: 12px">
            icon-12天后作业结束
          </text>
          <!--          作业详情和操作按钮 -->
          <view style="width: 100%;display: flex; margin-top: 20px;justify-content: space-between">
            <view>
              <view>
                <text style="font-size: 18px;">
                  0
                </text>
                /20
              </view>
              <view class="info">
                错题数/总数
              </view>
            </view>
            <view v-if="false" style="margin-left: 30rpx">
              <view style="color: #1ab270">
                <text style="font-size: 18px">
                  78
                </text>
                %
              </view>
              <view class="info">
                正确率
              </view>
            </view>
            <view class="btn-2">
              <button
                type="default" size="mini" plain style="border-radius: 20px;height: 35px;line-height: 35px"
                @click="assess"
              >
                评价
              </button>
              <button
                type="primary" size="mini"
                style="border-radius: 20px;height: 35px;line-height: 35px;margin-left: 8px;"
              >
                学情报告
              </button>
            </view>
          </view>
        </view>
      </view>
      <!--      空 -->
      <view v-else>
        <GuoduEmpty message="当天没有作业，休息休息吧" />
      </view>
      <!--      评价弹窗 -->
      <GuoduDrawer
        :visible="drawerVisible"
        :mask-click="maskClick"
        :mode="mode"
        size="40%"
        :radius="showRadius"
        @close="onDrawerClosed"
      >
        <template #header>
          <GuoduDrawerHeader title="作业评价" @close="drawerVisible = false" />
        </template>
        <view class="work-title">
          关于孔乙己的理解
        </view>
        <view>
          <view class="star-rating">
            <text>作业时长</text>
            <view
              v-for="(star, index) in stars"
              :key="index"
              class="star"
              :class="{ active: index < rating }"
              @click="setRating(index + 1)"
            >
              ★
            </view>
          </view>  <view class="star-rating">
            <text>难易度 </text>
            <view
              v-for="(star, index) in stars"
              :key="index"
              class="star"
              :class="{ active: index < rating }"
              @click="setRating(index + 1)"
            >
              ★
            </view>
          </view>  <view class="star-rating">
            <text>作业量 </text>
            <view
              v-for="(star, index) in stars"
              :key="index"
              class="star"
              :class="{ active: index < rating }"
              @click="setRating(index + 1)"
            >
              ★
            </view>
          </view>
        </view>
        <view style="display: flex;margin-top: 20px">
          <button type="default" plain style="width: 45%;" @click="drawerVisible = false">
            取消
          </button>
          <button type="primary" style="width: 45%" @click="drawerVisible = false">
            提交
          </button>
        </view>
      </GuoduDrawer>
    </view>
    <!--

      <view class="p-3">
        <button @tap="to('/pages/login/index')">
          To Login
        </button>
        <button @tap="handleLogout">
          Logout
        </button>
        <button @tap="to('/pages/icon/index')">
          To Icon
        </button>
        <button @tap="to('/pages/fetchPage/index')">
          To FetchPage
        </button>
      </view>
    -->
  </view>
</template>

<style lang="scss" scoped>
.work-title {
  font-size: 32rpx;
  color: #888888;
  margin-left: 24rpx;
}

.btn-2 {
  margin-top: 30rpx;
  display: flex;
  height: 60rpx;
  text-align: center;
  align-items: center

}

.info {
  margin-top: 10rpx;
}

.classify {
  display: flex;
  padding: 10rpx;
  font-size: 24rpx;
  background-color: #fff2dd;
  color: #ff950b;
  border-radius: 30rpx;
}

.card-title {
  display: flex;
  justify-content: space-between;
}

.left {
  display: flex;
}

.card {
  padding: 32rpx;
  border-radius: 32rpx;
  margin-top: 40rpx;
  width: 100%;
  height: 320rpx;
  background-color: #fff;
}

.subjectsName {
  margin-left: 20rpx;
}

.num {
  margin-right: 40rpx;
  width: 44rpx;
  height: 44rpx;
  text-align: center;
  background-color: white;
  border-radius: 50%;
}

.subjects {
  display: flex;
  flex-wrap: wrap;
  width: 100%;
  justify-content: space-between;
}

.subjects-item {
  margin-top: 40rpx;
  border-radius: 20rpx;
  width: 48%;
  height: 140rpx;
  background-color: #ffefde;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.subjects-item:nth-child(2) {
  background-color: #ddfdf2;
}

.subjects-item:nth-child(3) {
  background-color: #e5f0ff;
}

.subjects-item:nth-child(4) {
  background-color: #ecebff;
}

.container {
  padding: 20px;
}

.scroll-view {
  white-space: nowrap;
}

.date-item {
  display: inline-block;
  margin-right: 20px;
  text-align: center;
  padding: 6px;
  border-radius: 8px;
  height: 55px;
}

.date-item.selected {
  background-color: #2467cc;
  color: #fff;
}

.date {
  font-size: 18px;
  font-weight: bold;
  margin: 10px 10px;
}

.homework {
  font-size: 14px;
  display: block;
  margin-top: 5px;
  width: 50px;
}

.point {
  font-size: 30px;
  display: block;
  margin-top: -20px;
  color: #2467cc;
}

.noWork {
  background-color: #2467cc;
  color: #fff;
  height: 60px;
  width: 60px;
  border-radius: 50%;
}

.all {
  font-size: 16px;
  color: #999;
}

.mt-60rpx {
  margin-top: 0;
}

.list {
  margin-top: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
}

.star-rating {
  display: flex;
  font-size: 18px;
  align-items: center;
  margin-left: 20px;
}

.star {
  margin-left: 5px;
  font-size: 34px;
  color: #ccc;
  cursor: pointer;
}

.star.active {
  color: #ffcc00;
}
</style>
