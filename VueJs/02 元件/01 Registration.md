# 01 Registration 註冊

## 全域註冊

```javascript

import { createApp } from 'vue'

const app = createApp({})

app
.component('ComponentA', ComponentA)
.component('ComponentB', ComponentB)
.component('ComponentC', ComponentC)

```

> 之後可以在任何地方使用 ComponentA, ComponentB, ComponentC

## 區域註冊 (元件內部直接引入)

```vue
<script setup>
import ComponentA from './ComponentA.vue'
</script>

<template>
  <ComponentA />
</template>
```

## 元件命名原則

推薦使用大駝峰命名法

為什麼使用 PascalCase (大駝峰命名法) 命名 Vue 組件？

1.合法的 JavaScript 標識符：

- PascalCase 是合法的 JavaScript 標識符，這使得在 JavaScript 中導入和註冊組件都很容易。
- 使用 PascalCase 命名組件能夠讓 IDE 更好地提供自動補全功能，提升開發效率。

> PascalCase 是指使用大寫字母開頭的命名方式，例如 MyComponent。

2.區分 Vue 組件與原生 HTML 元素：

- 在模板中使用 <PascalCase /> 可以更清楚地表明這是一個 Vue 組件，而不是原生 HTML 元素。
- 這也能幫助區分 Vue 組件和自定義元素 (web components)，避免混淆。

3.DOM 內模板中的限制

- 在 DOM 內模板中，PascalCase 的標籤名是不可用的。這是因為 HTML 標準要求標籤名是大小寫不敏感的，而 PascalCase 不符合這一要求。
- 為了解決這個問題，Vue 支持將模板中使用的 kebab-case 標籤名解析為 PascalCase 註冊的組件。

> kebab-case 是指使用連字符 - 分隔的命名方式，例如 my-component。

```vue
<script>
import MyComponent from './MyComponent.vue';

export default {
  components: {
    MyComponent,
  },
};
</script>

<template>
  <div>
    <MyComponent />
    <my-component />
  </div>
</template>
```