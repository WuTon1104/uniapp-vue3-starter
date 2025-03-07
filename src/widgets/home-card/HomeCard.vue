<script setup>
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
function assess() {
  emit('assess')
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
      <view v-if="state" class="right">
        icon 已批改
      </view>
      <view v-else class="right">
        icon 未批改
      </view>
    </view>
    <!-- 剩余时间 -->
    <text style="color:#949698; font-size: 24rpx">
      icon-{{ time }}天后作业结束
    </text>
    <!-- 作业详情和操作按钮 -->
    <view style="width: 100%;display: flex; margin-top: 20px;justify-content: space-between">
      <view>
        <view>
          <text style="font-size: 36rpx;color: red">
            {{ error }}
          </text>
          /{{ total }}
        </view>
        <view class="info">
          错题数/总数
        </view>
      </view>
      <view v-if="state" style="margin-left: 30rpx">
        <view style="color: #1ab270">
          <text style="font-size: 36rpx">
            {{ accuracy }}
          </text>
          %
        </view>
        <view class="info">
          正确率
        </view>
      </view>
      <view class="btn-2">
        <button
          type="default" size="mini" plain style="border-radius: 40rpx;height: 70rpx;line-height: 70rpx"
          @click="assess"
        >
          评价
        </button>
        <button
          type="primary" size="mini"
          style="border-radius: 40rpx;height: 70rpx;line-height: 70rpx;margin-left: 16rpx;"
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
