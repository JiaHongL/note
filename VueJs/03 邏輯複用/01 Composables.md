# 01 Composables 組合函式

Vue 的 Composables 是 Vue 3 中一種新的設計模式，用來組織和重用邏輯。Composables 是基於 Composition API 的函式，可以讓我們在不同的元件間共享邏輯。

Composables 的特點：

- 組織邏輯：Composables 可以將某一個功能邏輯組織在一起，並且可以在多個元件中重用。
- 更靈活的組合：相較於 Mixins，Composables 提供了更靈活的組合方式，可以更細粒度地控制邏輯。
- 易於測試：因為 Composables 是純函式，所以更容易編寫單元測試。

> 純邏輯，不會包含 View。

## 使用範例

### useMouse 範例

```javascript
// mouse.js
import { ref, onMounted, onUnmounted } from 'vue'

// 按照慣例，組合式函數名以“use”開頭
export function useMouse() {
  // 被組合式函數封裝和管理的狀態
  const x = ref(0)
  const y = ref(0)

  // 組合式函數可以隨時更改其狀態。
  function update(event) {
    x.value = event.pageX
    y.value = event.pageY
  }

  // 一個組合式函數也可以掛靠在所屬組件的生命週期上
  // 來啟動和卸載副作用
  onMounted(() => window.addEventListener('mousemove', update))
  onUnmounted(() => window.removeEventListener('mousemove', update))

  // 通過返回值暴露所管理的狀態
  return { x, y }
}
```

> 默認 useXXX 為組合函數。

```vue
<script setup>
import { useMouse } from './mouse.js'

const { x, y } = useMouse()
</script>

<template>Mouse position is at: {{ x }}, {{ y }}</template>
```

### useEventListener 範例

```javascript
// event.js
import { onMounted, onUnmounted } from 'vue'

export function useEventListener(target, event, callback) {
  // 如果你想的話，
  // 也可以用字符串形式的 CSS 選擇器來尋找目標 DOM 元素
  onMounted(() => target.addEventListener(event, callback))
  onUnmounted(() => target.removeEventListener(event, callback))
}
```

```js
// mouse.js
import { ref } from 'vue'
import { useEventListener } from './event'

export function useMouse() {
  const x = ref(0)
  const y = ref(0)

  useEventListener(window, 'mousemove', (event) => {
    x.value = event.pageX
    y.value = event.pageY
  })

  return { x, y }
}
```

> 這裡的 useEventListener 是一個通用的組合函數，可以在任何元件中重用。
> 也會自動清理事件監聽器。

### useFetch 範例

```javascript
// fetch.js
import { ref } from 'vue'

export function useFetch(url) {
  const data = ref(null)
  const error = ref(null)

  fetch(url)
    .then((res) => res.json())
    .then((json) => (data.value = json))
    .catch((err) => (error.value = err))

  return { data, error }
}
```

```vue  
<script setup>
import { useFetch } from './fetch.js'
const { data, error } = useFetch('...')
</script>

<template>
  <div v-if="data">Data: {{ data }}</div>
  <div v-if="error">Error: {{ error }}</div>
</template>
```

### useModal 範例

```javascript
// modal.js
import { ref } from 'vue'

export function useModal() {
  const isOpen = ref(false)

  function open() {
    isOpen.value = true
  }

  function close() {
    isOpen.value = false
  }

  return { isOpen, open, close }
}
```

```vue
<script setup>
import { useModal } from './modal.js'
const { isOpen, open, close } = useModal()
</script>

<template>
  <button @click="open">Open Modal</button>
  <div v-if="isOpen">
    <div>Modal Content</div>
    <button @click="close">Close Modal</button>
  </div>
</template>
```

### 最佳實踐

- 組合函數的名稱以 use 開頭。
- 輸入參數應該使用 toValue 的方式，而不是直接使用。
- 返回值盡量使用 ref，才能在解構時獲取到響應式數據。
- 副作用，如事件監聽器，應該在 onMounted 和 onUnmounted 中處理。
- 只能在 setup 函數中使用組合函數。
- 組合函數應該是純函數，不應該包含任何與 View 相關的邏輯。

### 透過 Composables 重用邏輯，提高代碼可讀性和可維護性，提高靈活性。

```vue
<script setup>
import { useFeatureA } from './featureA.js'
import { useFeatureB } from './featureB.js'
import { useFeatureC } from './featureC.js'

const { foo, bar } = useFeatureA()
const { baz } = useFeatureB(foo)
const { qux } = useFeatureC(baz)
</script>
```