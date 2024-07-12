<template>
  <!-- 滚动吸附 -->
  <div class="layout" ref="scrollRef">
    <div id="part1" class="screen">
      <Fir />
    </div>
    <div id="part2" class="screen">
      <Sed />
    </div>
    <div id="part3" class="screen">
      <Thd />
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue';
import Fir from './fxyjFir.vue';
import Sed from './fxyjSed.vue';
import Thd from './fxyjThd.vue';

// 定义一个 ref，用于获取 layout 元素的引用
const scrollRef = ref(null);
let isScrolling = false;

// 定义平滑滚动函数
const smoothScroll = (targetPosition) => {
  isScrolling = true; // 标记正在滚动，防止重复触发
  const startPosition = scrollRef.value.scrollTop; // 获取当前滚动位置
  const distance = targetPosition - startPosition; // 计算目标位置与当前滚动位置的距离
  const duration = 500; // 动画持续时间
  let startTime = null;

  // 定义缓动函数
  const easeInOutQuad = (t, b, c, d) => {
    t /= d / 2;
    if (t < 1) return (c / 2) * t * t + b;
    t--;
    return (-c / 2) * (t * (t - 2) - 1) + b;
  };

  // 动画执行函数
  const animation = (currentTime) => {
    if (startTime === null) startTime = currentTime; // 初始化开始时间
    const timeElapsed = currentTime - startTime; // 计算经过的时间
    const run = easeInOutQuad(timeElapsed, startPosition, distance, duration); // 计算当前滚动位置
    scrollRef.value.scrollTop = run; // 设置滚动位置
    if (timeElapsed < duration) {
      requestAnimationFrame(animation); // 继续动画
    } else {
      isScrolling = false; // 动画结束，标记滚动结束
    }
  };

  requestAnimationFrame(animation); // 开始动画
};

// 处理滚动事件的函数
const handleScroll = (event) => {
  if (isScrolling) return; // 如果正在滚动，忽略事件

  const screenHeight = window.innerHeight; // 获取屏幕高度
  const scrollTop = scrollRef.value.scrollTop; // 获取当前滚动位置
  const currentScreen = Math.round(scrollTop / screenHeight); // 计算当前屏幕索引

  let targetScreen = currentScreen; // 初始化目标屏幕索引

  if (event.deltaY > 0 && currentScreen < 2) {
    // 向下滚动，并且当前不是最后一屏
    targetScreen = currentScreen + 1;
  } else if (event.deltaY < 0 && currentScreen > 0) {
    // 向上滚动，并且当前不是第一屏
    targetScreen = currentScreen - 1;
  }

  const targetPosition = targetScreen * screenHeight; // 计算目标位置
  smoothScroll(targetPosition); // 平滑滚动到目标位置
};

// 在组件挂载时添加滚动事件监听器
onMounted(() => {
  scrollRef.value.addEventListener('wheel', handleScroll);
});

// 在组件卸载时移除滚动事件监听器
onBeforeUnmount(() => {
  scrollRef.value.removeEventListener('wheel', handleScroll);
});
</script>

<style scoped>
.layout {
  position: relative;
  overflow: hidden;
  overflow-y: auto;
  height: 100vh;
  width: 100vw;
  scrollbar-width: none; /* Firefox */
}

.screen {
  width: 100%;
  height: 100vh;
}

#part1 {
  background: lightblue;
}

#part2 {
  background: lightgreen;
}

#part3 {
  background: lightcoral;
}
</style>
