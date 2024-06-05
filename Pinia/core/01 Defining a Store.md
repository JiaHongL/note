# 01 Defining a Store

## Option Stores

```js
export const useCounterStore = defineStore('counter', {
  state: () => ({ count: 0, name: 'Eduardo' }),
  getters: {
    doubleCount: (state) => state.count * 2,
  },
  actions: {
    increment() {
      this.count++
    },
  },
})
```

> state: 是設定 store 的初始狀態
> getters: 是 store 的計算屬性
> actions: 是 store 的方法

## Setup Stores

```js
export const useCounterStore = defineStore('counter', () => {
  const count = ref(0)
  const name = ref('Eduardo')
  const doubleCount = computed(() => count.value * 2)
  function increment() {
    count.value++
  }

  return { count, name, doubleCount, increment }
})
```
> 這種方式是使用 setup 函數來定義 store，這樣可以更好的利用 Composition API 的特性
> 這種方式也可以使用 `pinia` 的 `useStore` 來取得 store
> ref 就是設定 store 的初始狀態
> computed 就是 store 的計算屬性
> function 就是 store 的方法

## Setup api 可以使用任何 provide 的值

```js
import { inject } from 'vue'
import { useRoute } from 'vue-router'

export const useSearchFilters = defineStore('search-filters', () => {
  const route = useRoute()
  // 这里假定 `app.provide('appProvided', 'value')` 已经调用过
  const appProvided = inject('appProvided')

  // ...

  return {
    // ...
  }
})
```

## 使用 store

```vue
<script setup>
import { useCounterStore } from '@/stores/counter'
// 可以在组件中的任意位置访问 `store` 变量 ✨
const store = useCounterStore()
</script>
```

## 從 Store 解構

```vue
<script setup>
import { storeToRefs } from 'pinia'
const store = useCounterStore();
// 可以獲得 name 和 doubleCount 的 ref
const { name, doubleCount } = storeToRefs(store)
// 也可以獲得 increment 方法
const { increment } = store
</script>
```