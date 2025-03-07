<script setup>
import { computed, ref } from 'vue'

const props = defineProps({
  label: {
    type: String,
    default: '',
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

// 评分
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
  <view class="flex flex-items-center mt-2">
    <text>&nbsp;&nbsp;&nbsp;{{ label }}</text>
    <view
      v-for="index in stars"
      :key="index"
      class="star ml-2 text-xl"
      :class="{ active: index < rating, yellow: isYellow, red: !isYellow }"
      @click="setRating(index + 1)"
    >
      ★
    </view>
    <text class="ml-3 text-slate-400">
      {{ ratingText }}
    </text>
  </view>
</template>

<style scoped>
.star {
  color: #ccc; /* 默认灰色 */
}

.star.active.yellow {
  color: #ffcc00; /* 超过60%为黄色 */
}

.star.active.red {
  color: #ff0000; /* 不超过60%为红色 */
}
</style>
