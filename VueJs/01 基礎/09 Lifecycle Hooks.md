# 09 Lifecycle Hooks

## 1. Introduction

![alt text](/VueJs/images/life-cycle.png)

### Composition API 的生命週期鉤子

- setup：在組件初始化時執行，用於定義組件的狀態和行為，返回值會暴露給模板。這個鉤子是在所有其他生命週期鉤子之前執行的。
- onBeforeMount：在組件即將掛載到 DOM 之前執行。此時模板已經編譯完成，但尚未插入 DOM。
- onMounted：在組件掛載到 DOM 之後執行。此時可以操作 DOM。
- onBeforeUpdate：在組件的**響應式數據改變**，並且即將**重新渲染之前**執行。此時**數據已經改變**，但**DOM還沒有更新**。
- onUpdated：在**組件重新渲染並更新 DOM 之後執行**。此時新的 DOM 已經基於最新的響應式數據進行了更新。
- onBeforeUnmount：在組件即將從 DOM 中卸載之前執行。這個鉤子常用於清理工作，例如移除事件監聽器。
- onUnmounted：在組件從 DOM 中卸載之後執行。此時組件的所有副作用都已經移除。
  
### Options API 的生命週期鉤子

- beforeCreate：在組件實例初始化之前執行。這個鉤子在 setup 之前觸發，但在 Composition API 中一般不需要使用它。
- created：在組件實例已經創建之後執行。此時 data 已經被初始化，但組件尚未掛載到 DOM。
- beforeMount：在組件掛載到 DOM 之前執行。此時模板已經編譯完成，但尚未插入 DOM。
- mounted：在組件掛載到 DOM 之後執行。此時可以操作 DOM。
- beforeUpdate：在組件的響應式數據改變，並且即將重新渲染之前執行。此時數據已經改變，但 DOM 還沒有更新。
- updated：在組件重新渲染並更新 DOM 之後執行。此時新的 DOM 已經基於最新的響應式數據進行了更新。
- beforeUnmount：在組件即將從 DOM 中卸載之前執行。這個鉤子常用於清理工作，例如移除事件監聽器。
- unmounted：在組件從 DOM 中卸載之後執行。此時組件的所有副作用都已經移除。

### nextTick

- nextTick 函數的回調函數會在當前更新週期的 DOM 更新之後執行。

```vue
<template>
  <div>
    <p ref="message">{{ message }}</p>
    <button @click="updateMessage">Update Message</button>
  </div>
</template>

<script setup>
import { ref, nextTick } from 'vue';

const message = ref('Hello, Vue 3!');

const updateMessage = async () => {
  message.value = 'Hello, nextTick!';
  await nextTick();
  console.log('DOM has updated:', message.value);
};
</script>
```