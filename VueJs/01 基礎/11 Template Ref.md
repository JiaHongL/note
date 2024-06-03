# 11 Template Ref

## ref attribute

```template
<script setup>
import { ref, onMounted } from 'vue'

// 聲明一個 ref 來存放該元素的引用
// 必須和模板裡的 ref 同名
const input = ref(null)

onMounted(() => {
  input.value.focus()
})
</script>

<template>
  <input ref="input" />
</template>
```

> ref 屬性可以用來獲取元素的引用

## 如果不使用 <script setup>，必須在 setup() 裡聲明 ref，並返回。

```js
export default {
  setup() {
    const input = ref(null)
    // ...
    return {
      input
    }
  }
}
```

## 在初次渲染時 input.value 為 null，所以需要在 onMounted() 裡設置 focus。

```vue
<script setup>
import { ref, onMounted } from 'vue'

const input = ref(null)

onMounted(() => {
  input.value.focus()
})

watchEffect(() => {
  if (input.value) {
    console.log('input 已經掛載') 
  } else {
    console.log('input 未掛載')
    // 此時還未掛載，或此元素已經被卸載（例如通過 v-if 控制）
  }
})
</script>

<template>
  <input ref="input" />
</template>
```
