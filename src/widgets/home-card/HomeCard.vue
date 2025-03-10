<script setup>
// 定义 props
defineProps({
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
    default: '12',
  },
  accuracy: {
    type: Number,
    default: 0,
  },
})

// 定义事件
const emit = defineEmits(['assess'])

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
        <view class="font-medium" style="margin-left: 20rpx">
          {{ title }}
        </view>
      </view>
      <view v-if="state" class="right text-sm">
        icon 已批改
      </view>
      <view v-else class="right">
        icon 未批改
      </view>
    </view>
    <!-- 剩余时间 -->
    <text class="text-xs" style="color:#949698;">
      icon-{{ time }}天后作业结束
    </text>
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
