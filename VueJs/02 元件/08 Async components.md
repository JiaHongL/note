# 08 Async Components

## 基本用法

```js
import { defineAsyncComponent } from 'vue'

const AsyncComp = defineAsyncComponent(() => {
  return new Promise((resolve, reject) => {
    // ...從服務器獲取組件
    resolve(/* 獲取到的組件 */)
  })
})
// ... 像使用其他一般組件一樣使用 `AsyncComp`
```

## ES 模塊

```js
import { defineAsyncComponent } from 'vue'

const AsyncComp = defineAsyncComponent(() =>
  import('./components/MyComponent.vue')
)
// ... 像使用其他一般組件一樣使用 `AsyncComp`
```

## 全局註冊

```js
import { createApp } from 'vue'

const app = createApp({})
app.component('async-component', defineAsyncComponent(() =>
  import('./components/MyComponent.vue')
))
app.mount('#app')
```

## 元件內部直接使用

```vue  
<script setup>
import { defineAsyncComponent } from 'vue'

const AdminPage = defineAsyncComponent(() =>
  import('./components/AdminPageComponent.vue')
)
</script>

<template>
  <AdminPage />
</template>
```

> 被使用的時候，才會加載 `AdminPageComponent.vue` 組件。

## 加載與錯誤狀態

```js
const AsyncComp = defineAsyncComponent({
  // 加載函數
  loader: () => import('./Foo.vue'),

  // 加載異步組件時使用的組件
  loadingComponent: LoadingComponent,
  // 展示加載組件前的延遲時間，默認為 200ms
  delay: 200,

  // 加載失敗後展示的組件
  errorComponent: ErrorComponent,
  // 如果提供了一個 timeout 時間限制，並超時了
  // 也會顯示這裡配置的報錯組件，默認值是：Infinity
  timeout: 3000
})
```