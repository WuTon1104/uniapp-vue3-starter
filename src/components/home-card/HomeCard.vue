<script setup>
import { useTimeAgo } from '../../composables/useTimeAgo'

// 定义 props
const props = defineProps({
  state: {
    type: Boolean,
    default: false,
  },
  error: {
    type: Number,
    default: 5,
  },
  total: {
    type: Number,
    default: 15,
  },
  type: {
    type: String,
    default: '语文基础',
  },
  title: {
    type: String,
    default: '',
  },
  time: {
    type: String,
    default: '1741933860', // 秒级时间戳
  },
  accuracy: {
    type: Number,
    default: 0,
  },
})

// 定义事件
const emit = defineEmits(['assess'])
// 将 props.time 转换为响应式引用
const time = toRef(props, 'time')

// 使用 useTimeAgo
const timeAgo = useTimeAgo(Number(time.value) * 1000, {
  messages: {
    justNow: '刚刚',
    past: n => `${n} 前`,
    future: n => `${n} 后`,
  },
})

// 评价按钮点击事件
function assess(title) {
  emit('assess', title)
}
</script>

<template>
  <view class="card">
    <!-- 分类 题目 状态 -->
    <view class="card-title">
      <view class="left">
        <view
          class="classify"
          :class="{
            chinese: type === '语文基础',
            math: type === '数学分层',
            english: type === '英语基础',
            history: type === '历史基础',
          }"
        >
          <text>{{ type }}</text>
        </view>
        <view style="margin-left: 20rpx">
          {{ title }}
        </view>
      </view>
      <view v-if="state" class="right flex flex-items-center text-sm">
        <svg style="margin-right: 6rpx" xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 12 12"><!-- Icon from Garden SVG Icons by Zendesk - https://github.com/zendeskgarden/svg-icons/blob/main/LICENSE.md --><path fill="none" stroke="#0055ff" stroke-linecap="round" stroke-linejoin="round" d="m1 7l3 3l7-7" /></svg>        已批改
      </view>
      <view v-else class="right flex flex-items-center">
        <svg t="1741581195695" class="icon mr-1" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="5041" width="16" height="16"><path d="M512 64c-247.039484 0-448 200.960516-448 448S264.960516 960 512 960 960 759.039484 960 512 759.039484 64 512 64zM512 832.352641c-26.496224 0-48.00043-21.504206-48.00043-48.00043 0-26.496224 21.504206-48.00043 48.00043-48.00043s48.00043 21.504206 48.00043 48.00043S538.496224 832.352641 512 832.352641zM600.576482 505.184572c-27.839699 27.808735-56.575622 56.544658-56.575622 82.368284l0 54.112297c0 17.664722-14.336138 32.00086-32.00086 32.00086s-32.00086-14.336138-32.00086-32.00086l0-54.112297c0-52.352533 39.999785-92.352318 75.32751-127.647359 25.887273-25.887273 52.67249-52.639806 52.67249-73.984034 0-53.343368-43.07206-96.735385-95.99914-96.735385-53.823303 0-95.99914 41.535923-95.99914 94.559333 0 17.664722-14.336138 31.99914-32.00086 31.99914s-32.00086-14.336138-32.00086-31.99914c0-87.423948 71.775299-158.559333 160.00086-158.559333s160.00086 72.095256 160.00086 160.735385C672.00086 433.791157 635.680581 470.080473 600.576482 505.184572z" fill="#fcc78e" p-id="5042" data-spm-anchor-id="a313x.search_index.0.i0.4bca3a81jG945n" class="selected" /></svg>
        未批改
      </view>
    </view>
    <!-- 剩余时间 -->
    <view class="mt-2 flex text-xs" style="color:#949698;">
      <svg t="1741581438713" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="6308" width="14" height="14"><path d="M1008.185895 221.122824c-57.098377-90.39743-136.996106-165.195304-230.893437-216.293851-19.299451-10.499702-43.398766-3.299906-53.79847 15.999545s-3.299906 43.398766 15.899548 53.798471c82.097666 44.598732 151.795685 109.996873 201.694266 188.994627 7.599784 11.999659 20.39942 18.499474 33.599045 18.499474 7.299792 0 14.599585-1.999943 21.199397-6.099827 18.499474-11.799665 24.099315-36.298968 12.299651-54.898439zM468.201245 336.919532c-21.899377 0-39.698871 17.799494-39.698871 39.698872v175.994996c0.299991 10.699696 4.899861 20.299423 11.999659 27.199227l79.097751 90.097439c7.799778 8.899747 18.799466 13.499616 29.799153 13.499616 9.299736 0 18.599471-3.299906 26.199255-9.899718 16.499531-14.499588 18.099485-39.498877 3.599898-55.998408l-71.397971-81.297689V376.618404c0.099997-21.899377-17.699497-39.698871-39.598874-39.698872z" fill="#d9ddde" p-id="6309" /><path d="M507.900117 85.226687V43.127884c0-21.899377-17.799494-39.698871-39.698872-39.698871s-39.698871 17.799494-39.698871 39.698871v43.998749c-28.299196 3.099912-56.698388 8.79975-84.897587 17.399506-247.99295 74.897871-388.788948 337.590403-313.891077 585.583353 6.399818 20.999403 28.49919 32.799068 49.498593 26.499246 20.999403-6.299821 32.899065-28.49919 26.499247-49.498592C43.413321 461.016004 160.409995 242.72221 366.504136 180.523978c206.094141-62.198232 424.387936 54.698445 486.586168 260.792586 62.298229 206.094141-54.698445 424.387936-260.792587 486.586168-148.495779 44.898724-309.491202-2.399932-410.088342-120.496575-14.199596-16.699525-39.298883-18.699468-55.998408-4.499872-16.699525 14.199596-18.699468 39.298883-4.499872 55.998408C212.608511 965.301669 344.004776 1024 479.500924 1024c45.298712 0 91.09741-6.599812 135.796139-20.099429 247.99295-74.997868 388.68895-337.6904 313.79108-585.583353C870.7898 225.12271 698.494698 97.026352 507.900117 85.226687z" fill="#d9ddde" p-id="6310" /></svg> &nbsp;
            {{ time }}天后作业结束
<!--      {{ timeAgo }}天后作业结束-->
    </view>
    <!-- 作业详情和操作按钮 -->
    <view style="width: 100%;display: flex; margin-top: 60rpx;justify-content: space-between">
      <view>
        <view>
          <text style="font-size: 36rpx;color: red">
            {{ error }}
          </text>
          /{{ total }}
        </view>
        <view class="info text-xs">
          错题数/总数
        </view>
      </view>
      <view v-if="state" style="margin-left: -1rem">
        <view style="color: #1ab270">
          <text style="font-size: 36rpx">
            {{ accuracy }}
          </text>
          %
        </view>
        <view class="info text-xs">
          正确率
        </view>
      </view>
      <view class="btn-2">
        <button
          class="bt"
          type="default" size="mini" plain
          @click="assess(title)"
        >
          评价
        </button>
        <button
          class="bt"
          type="primary" size="mini"
        >
          学情报告
        </button>
      </view>
    </view>
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
.chinese{
  background-color: #fff2dd;
  color: #ff950b;
}
.math{
  background-color: #e5f0ff;
  color: #50b7ff;
}
.english{
  background-color: #ddfdf2;
  color: #1abd7c;
}
.history{
  background-color: #8e85ff;
  color: #ecebff;
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
  padding: 40rpx;
}

.scroll-view {
  white-space: nowrap;
}

.date-item {
  display: inline-block;
  margin-right: 40rpx;
  text-align: center;
  padding: 12rpx;
  border-radius: 16rpx;
  height: 110rpx;
}

.date-item.selected {
  background-color: #2467cc;
  color: #fff;
}

.date {
  font-size: 36rpx;
  font-weight: bold;
  margin: 20rpx 20rpx;
}

.homework {
  font-size: 28rpx;
  display: block;
  margin-top: 10rpx;
  width: 100rpx;
}

.point {
  font-size: 60rpx;
  display: block;
  margin-top: -40rpx;
  color: #2467cc;
}

.noWork {
  background-color: #2467cc;
  color: #fff;
  height: 120rpx;
  width: 120rpx;
  border-radius: 50%;
}

.all {
  font-size: 32rpx;
  color: #999;
}

.mt-60rpx {
  margin-top: 0;
}

.list {
  margin-top: 20rpx;
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
}

.star-rating {
  display: flex;
  font-size: 36rpx;
  align-items: center;
  margin-left: 40rpx;
}

.star {
  margin-left: 10rpx;
  font-size: 68rpx;
  color: #ccc;
  cursor: pointer;
}

.star.active {
  color: #ffcc00;
}
.bt{
    border-radius: 40rpx;
    height: 70rpx;
    line-height: 70rpx;
    margin-left: 16rpx;
}
</style>
