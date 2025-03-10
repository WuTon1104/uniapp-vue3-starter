<script setup lang="ts">
const dates = ref([
  {
    date: '1',
    homework: 1,
    work: [{ error: 5, true: 15, type: '语文基础', title: '关于孔乙己的理解', time: '6', state: true }],
  },
  {
    date: '2',
    homework: 2,
    work:
      [{ error: 2, true: 15, type: '数学分层', title: '一元二次方程的解答', time: '10', state: false }, {
        error: 5,
        true: 15,
        type: '语文基础',
        title: '关于孔乙己的理解2',
        time: '18',
        state: true,
      }],
  },
  { date: '3', homework: 0 },
  {
    date: '4',
    homework: 3,
    work:
      [{ error: 2, true: 15, type: '英语基础', title: 'SD', time: '11', state: false }, {
        error: 5,
        true: 15,
        type: '语文基础',
        title: '关于孔乙己的理解2',
        time: '12',
        state: true,
      }, {
        error: 7,
        true: 13,
        type: '英语基础',
        title: 'ABC',
        time: '11',
        state: true,
      }],
  },
  {
    date: '5',
    homework: 1,
    work: [{ error: 5, true: 15, type: '历史基础', title: '王朝的更替', time: '4', state: true }],
  },
  { date: '6', homework: 0 },
  { date: '7', homework: 0 },
])
const selectedIndex = ref(0) // 用于记录当前选中的项

function selectItem(index) {
  selectedIndex.value = index // 更新选中的项
}

const drawerVisible = ref(false)
const drawerVisibleDate = ref(false)
const maskClick = ref(true)
const showRadius = ref(true)
const now = new Date().getTime() ? new Date(new Date().getTime()) : new Date()
const year = ref(now.getFullYear())
const month = ref(now.getMonth() + 1)
// const selectDate = ref(`${year.value}/${month.value}`)

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

const tit = ref('')
// 触发评价
function assess(title) {
  tit.value = title
  drawerVisible.value = true
}

function onDrawerClosed() {
  drawerVisible.value = false
  uni.showToast({
    title: '关闭',
    icon: 'none',
  })
}
// 按下选择日期确定键 处理获得的数据
function onSub(e: { 0: number, 1: number }): { start_date: string, end_date: string } {
  // 将时间戳转换为 Date 对象
  const startDate = new Date(e[0])
  const endDate = new Date(e[1])

  // 提取年月日
  const startYear = startDate.getFullYear()
  const startMonth = startDate.getMonth() + 1 // 月份从 0 开始，需要加 1
  const startDay = startDate.getDate()

  const endYear = endDate.getFullYear()
  const endMonth = endDate.getMonth() + 1 // 月份从 0 开始，需要加 1
  const endDay = endDate.getDate()

  // 格式化日期为 YYYY-MM-DD
  const formatDate = (year: number, month: number, day: number): string => {
    return `${year}-${month.toString().padStart(2, '0')}-${day.toString().padStart(2, '0')}`
  }

  // 返回日期范围
  // return {
  //   start_date: formatDate(startYear, startMonth, startDay),
  //   end_date: formatDate(endYear, endMonth, endDay),
  // }

  console.log(formatDate(startYear, startMonth, startDay), formatDate(endYear, endMonth, endDay))
  drawerVisibleDate.value = false
}
</script>

<template>
  <view style="background-color: #f6f5fa; min-height: 100vh">
    <view class="top-bg p-3">
      <view class="mt-4 text-xl font-bold">
        作业列表
      </view>
      <view class="text-align-center">
        <text @click="handleChangeLastMonth"><&nbsp;</text>
        <text @click="drawerVisibleDate = true">{{ year }}/{{month}}</text>

        <text @click="handleChangeNextMonth">&nbsp;></text>
      </view>
      <!-- 头部 -->
      <GuoduDrawer
        :visible="drawerVisibleDate"
        :mask-click="maskClick"
        mode="top"
        size="60%"
        :radius="showRadius"
        @close="drawerVisibleDate = false"
      >
        <template #header>
          <GuoduDrawerHeader
            title="选择日期"
            @close="drawerVisibleDate = false"
          />
        </template>
        <GuoduCalendar
          :default-time="now.value"
          mode="week"
          :value="0"
          :start-week="0"
          @submit="onSub"
          @cancel="drawerVisibleDate = false"
        />
      </GuoduDrawer>

      <view style="margin-top: 28rpx">
        <scroll-view :show-scrollbar="false" scroll-x="true" class="whitespace-nowrap">
          <view class="flex">
            <view
              v-for="(item, index) in dates" :key="index" class="inline-block h-110rpx rounded-xl p-10rpx text-align-center"
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
      <view v-if="dates[selectedIndex].homework" class="mt-5 w-full flex flex-wrap items-center justify-between">
        <view>
          <view class="mt-60rpx text-xl font-semibold">
            <view class="flex items-center flex-justify-between" style="width: 93vw;">
              <view class="flex items-end">
                <view style="font-size: 60rpx;position: relative;top: -2rpx">
                  {{ dates[selectedIndex].homework }}
                </view>
                份作业
              </view>
              <text class="text-xs text-slate-400 font-normal">
                全部学科 >
              </text>
            </view>
          </view>
          <!--      单元格头 -->
        </view>
        <!--     默认显示四个 语数外历史 -->
        <view class="w-full flex flex-wrap justify-between">
          <view class="subjects-item">
            <view class="ml-2 flex flex-items-center">
              <view style="margin-right: 10rpx; display:flex; justify-content: space-around;align-items: center; text-align: center; width: 60rpx; height: 60rpx;background-color: #ffb151;border-radius: 50% 50%;">
                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 32 32"><!-- Icon from Carbon by IBM - undefined --><path fill="#ffffff" d="M19 10h7v2h-7zm0 5h7v2h-7zm0 5h7v2h-7zM6 10h7v2H6zm0 5h7v2H6zm0 5h7v2H6z"/><path fill="#ffffff" d="M28 5H4a2 2 0 0 0-2 2v18a2 2 0 0 0 2 2h24a2 2 0 0 0 2-2V7a2 2 0 0 0-2-2M4 7h11v18H4Zm13 18V7h11v18Z"/></svg>
              </view>
              语文
            </view>
            <view class="num">
              2
            </view>
          </view>
          <view class="subjects-item">
            <view class="ml-2 flex flex-items-center">
              <view style="margin-right: 10rpx; display:flex; justify-content: space-around;align-items: center; text-align: center; width: 60rpx; height: 60rpx;background-color: #1cbc7b;border-radius: 50% 50%;">
                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 32 32"><!-- Icon from Carbon by IBM - undefined --><path fill="#ffffff" d="M4 28h4v2H4a2 2 0 0 1-2-2v-4h2zm7-15v8H6a2 2 0 0 1-2-2v-2a2 2 0 0 1 2-2h3v-2H5v-2h4c1.103 0 2 .897 2 2m-2 4H6v2h3zM4 4h4V2H4a2 2 0 0 0-2 2v4h2zm24-2h-4v2h4v4h2V4a2 2 0 0 0-2-2m0 26h-4v2h4a2 2 0 0 0 2-2v-4h-2zm0-7v-2h-4v-6h4v-2h-4c-1.102 0-2 .897-2 2v6c0 1.103.898 2 2 2zm-8-8v6c0 1.103-.897 2-2 2h-5V8h2v3h3c1.103 0 2 .897 2 2m-2 0h-3v6h3z"/></svg>
              </view>
              英语
            </view>
            <view class="num">
              1
            </view>
          </view>
          <view class="subjects-item">

            <view class="ml-2 flex flex-items-center">
              <view style="margin-right: 10rpx; display:flex; justify-content: space-around;align-items: center; text-align: center; width: 60rpx; height: 60rpx;background-color: #3f98fa;border-radius: 50% 50%;">
                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 32 32"><!-- Icon from Carbon by IBM - undefined --><path fill="#ffffff" d="M11 19v2H5v-2h2v-5H5v-2h2v-1h2v8zm8 0h-4v-2h2c1.103 0 2-.897 2-2v-2c0-1.103-.897-2-2-2h-4v2h4v2h-2c-1.103 0-2 .897-2 2v4h6zm6-8h-4v2h4v2h-3v2h3v2h-4v2h4c1.103 0 2-.897 2-2v-6c0-1.103-.897-2-2-2M2 4v4h2V4h4V2H4a2 2 0 0 0-2 2m26-2h-4v2h4v4h2V4a2 2 0 0 0-2-2M4 28v-4H2v4a2 2 0 0 0 2 2h4v-2zm24-4v4h-4v2h4a2 2 0 0 0 2-2v-4z"/></svg>              </view>
              数学
            </view>
            <view class="num">
              1
            </view>
          </view>
          <view class="subjects-item">

            <view class="ml-2 flex flex-items-center">
              <view style="margin-right: 10rpx; display:flex; justify-content: space-around;align-items: center; text-align: center; width: 60rpx; height: 60rpx;background-color: #8d85fe;border-radius: 50% 50%;">
                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 32 32"><!-- Icon from Carbon by IBM - undefined --><path fill="#ffffff" d="M27.527 2.709A2 2 0 0 0 26 2h-2.69a1.5 1.5 0 0 0-1.343.83L21.382 4H20v-.5A1.5 1.5 0 0 0 18.5 2h-5A1.5 1.5 0 0 0 12 3.5V4h-1.382l-.585-1.17A1.5 1.5 0 0 0 8.69 2H6a2 2 0 0 0-1.972 2.333L4.732 8.5A3 3 0 0 0 7.69 11H8v12a3.003 3.003 0 0 0-3 3v4h22v-4a3.003 3.003 0 0 0-3-3V11h.31a3 3 0 0 0 2.958-2.5l.704-4.167a2 2 0 0 0-.445-1.624M25 26v2H7v-2a1 1 0 0 1 1-1h16a1 1 0 0 1 1 1m-3-3H10V11h12Zm3.296-14.833A1 1 0 0 1 24.31 9H7.69a1 1 0 0 1-.986-.833L6 4h2.382l1 2H14V4h4v2h4.617l1.001-2H26Z"/></svg>              </view>
              历史
            </view>
            <view class="num">
              1
            </view>
          </view>
        </view>
        <view />
        <HomeCard
          v-for="(item, index) in dates[selectedIndex].work"
          :key="index"
          :state="item.state"
          :homework="item.homework"
          :error="item.error"
          :total="item.true + item.error"
          :title="item.title"
          :type="item.type"
          :time="item.time"
          :accuracy="(item.true / (item.true + item.error)) * 100"
          @assess="assess"
        />
      </view>
      <!--      空 -->
      <view v-else>
        <GuoduEmpty message="当天没有作业，休息休息吧" />
      </view>
      <!--      评价弹窗 -->
      <GuoduDrawer
        :visible="drawerVisible"
        :mask-click="maskClick"
        mode="bottom"
        size="40%"
        :radius="showRadius"
        @close="onDrawerClosed"
      >
        <template #header>
          <GuoduDrawerHeader title="作业评价" @close="drawerVisible = false" />
        </template>
        <view class="ml-3 text-base text-slate-400">
          {{ tit }}
        </view>
        <view>
          <Star label="作业时长" :max-stars="5" />
          <Star label="难易度" :max-stars="5" />
          <Star label="作业量" :max-stars="5" />
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
  </view>
</template>

<style lang="scss" scoped>
.num {
  margin-right: 40rpx;
  width: 44rpx;
  height: 44rpx;
  text-align: center;
  background-color: white;
  border-radius: 50%;
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

.date {
  font-size: 18px;
  font-weight: bold;
  margin: 10px 10px;
}

.homework {
  font-size: 20rpx;
  margin-top: 5px;
  width: 50px;
}

.point {
  font-size: 30px;
  display: block;
  margin-top: -20px;
  color: #178fff;
}

.selected {
  background-color: #178fff;
  color: #fff;
  font-size: 14px;
}

.noWork {
  background-color: #178fff;
  color: #fff;
  height: 80rpx;
  width: 80rpx;
  border-radius: 50%;
}

.mt-60rpx {
  margin-top: 0;
}
</style>
