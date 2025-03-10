<script setup>
const props = defineProps({
  label: {
    type: String,
    default: '作业量',
  },
  maxStars: {
    type: Number,
    default: 5,
  },
})

const rating = ref(0)
const stars = ref(Array.from({ length: props.maxStars }, (_, i) => i + 1))

// 判断是否超过60%
const isYellow = computed(() => {
  return (rating.value / props.maxStars) > 0.6
})

// 设置评分
function setRating(value) {
  rating.value = value
}

// 根据评分显示文字
const ratingText = computed(() => {
  const percentage = (rating.value / props.maxStars) * 100
  if (percentage > 80) {
    return '非常满意'
  }
  else if (percentage > 60) {
    return '满意'
  }
  else {
    return '不满意'
  }
})
</script>

<template>
  <view class="star-rating-container">
    <text class="label font-normal">
      {{ label }}
    </text>
    <view class="star-rating">
      <view
        v-for="(star, index) in stars"
        :key="index"
        class="star"
        :class="{ active: index < rating, yellow: isYellow, red: !isYellow }"
        @click="setRating(index + 1)"
      >
        ★
      </view>
    </view>
    <text class="rating-text">
      {{ ratingText }}
    </text>
  </view>
</template>

<style scoped>
.star-rating-container {
  margin-top: 32rpx ;
  display: flex;
  align-items: center;
  margin-bottom: 20rpx;
}

.label {
  text-align-last: justify;
  width: 160rpx;
  text-align: right;
  margin: 0 24rpx;
  font-size: 32rpx;
  color: #333;
}

.star-rating {
  display: flex;
  align-items: center;
}

.star {
  cursor: pointer;
  font-size: 48rpx;
  color: #ccc; /* 默认灰色 */
  margin-right: 10rpx;
}

.star.active.yellow {
  color: #ffcc00; /* 超过60%为黄色 */
}

.star.active.red {
  color: #ff0000; /* 不超过60%为红色 */
}

.rating-text {
  margin-left: 20rpx;
  font-size: 32rpx;
  color: #333;
}
</style>
