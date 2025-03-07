<script setup lang="ts">
const dates = ref([
  {
    date: '1',
    homework: 1,
    work: [{ error: 5, true: 15, type: '语文基础', title: '关于孔乙己的理解', time: '12', state: true }],
  },
  {
    date: '2',
    homework: 2,
    work:
      [{ error: 2, true: 15, type: '数学分层', title: '一元二次方程的解答', time: '12', state: false }, {
        error: 5,
        true: 15,
        type: '语文基础',
        title: '关于孔乙己的理解2',
        time: '12',
        state: true,
      }],
  },
  { date: '3', homework: 0 },
  {
    date: '4',
    homework: 3,
    work:
      [{ error: 2, true: 15, type: '英语基础', title: 'SD', time: '12', state: false }, {
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
        time: '12',
        state: true,
      }],
  },
  {
    date: '5',
    homework: 1,
    work: [{ error: 5, true: 15, type: '历史基础', title: '王朝的更替', time: '12', state: true }],
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
const selectDate = ref('< 2025-03 >')

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
      <view class="text-align-center" @click="drawerVisibleDate = true">
        {{ selectDate }}
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
          mode="week"
          :value="0"
          :start-week="0"
          @submit="onSub"
          @cancel="drawerVisibleDate = false"
        />
      </GuoduDrawer>

      <view class="p-40rpx">
        <scroll-view :show-scrollbar="false" scroll-x="true" class="whitespace-nowrap">
          <view class="flex">
            <view
              v-for="(item, index) in dates" :key="index" class="mr-20rpx inline-block h-110rpx rounded-xl p-12rpx text-align-center"
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
      <view v-if="dates[selectedIndex].homework" class="mt-1 w-full flex flex-wrap items-center justify-between">
        <view>
          <view class="mt-60rpx text-xl font-bold">
            <view class="flex flex-justify-between items-center" style="width: 93vw">
              <view class="flex">
                <text style="font-size: 60rpx">
                  {{ dates[selectedIndex].homework }}
                </text>
                份作业
              </view>
              <text class="text-xs text-slate-400">
                全部学科 >
              </text>
            </view>
          </view>
          <!--      单元格头 -->
        </view>
        <!--     默认显示四个 语数外历史 -->
        <view class="w-full flex flex-wrap justify-between">
          <view class="subjects-item">
            <view class="ml-2">
              icon--语文
            </view>
            <view class="num">
              2
            </view>
          </view>
          <view class="subjects-item">
            <view class="ml-2">
              icon--英语
            </view>
            <view class="num">
              1
            </view>
          </view>
          <view class="subjects-item">
            <view class="ml-2">
              icon--数学
            </view>
            <view class="num">
              1
            </view>
          </view>
          <view class="subjects-item">
            <view class="ml-2">
              icon--历史
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

.selected {
  background-color: #2467cc;
  color: #fff;
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
  color: #2467cc;
}

.noWork {
  background-color: #2467cc;
  color: #fff;
  height: 60px;
  width: 60px;
  border-radius: 50%;
}

.mt-60rpx {
  margin-top: 0;
}
</style>
