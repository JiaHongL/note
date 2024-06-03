# 02 Prop

## Props Declaration

```vue
<script setup>
import { defineProps } from 'vue'

const props = defineProps({
  // 基本類型檢查
  propA: Number,
  // 多種可能的類型
  propB: [String, Number],
  // 必須的字串
  propC: {
    type: String,
    required: true
  },
  // 有預設值的數字
  propD: {
    type: Number,
    default: 100
  },
  // 有預設值的物件
  propE: {
    type: Object,
    // 物件或陣列的預設值必須由工廠函式返回
    default: () => {
      return { message: 'hello' }
    }
  },
  // 自定義驗證函式
  propF: {
    validator(value) {
      return value > 10
    }
  }
});

console.log(props.propA)

</script>

<template>
  <div>
    <p>{{ props.propA }}</p>
    <p>{{ props.propB }}</p>
    <p>{{ props.propC }}</p>
    <p>{{ props.propD }}</p>
    <p>{{ props.propE }}</p>
    <p>{{ props.propF }}</p>
  </div>

</template>
```

> null 和 undefined 被視為任意類型。這意味著如果 prop 的類型是 null 或 undefined，則可以接受任何類型的數據。
> 在 html 中使用 props 時，不需要加上 props 前綴，但建議加上以增加可讀性。

## component binding prop

```vue
<script setup>
defineProps({
  greetingMessage: String
})
</script>
<template>
  <div>
    <p>{{ greetingMessage }}</p>
  </div>
</template>
```

```vue
<template>
  <ComponentA greeting-message="Hello, Vue 3!" />
</template>
```

> 推薦使用 kebab-case 來命名 prop，可以區分 HTML 特性和 Vue prop。

## 靜態 vs 動態 prop

```vue
<template>
  <ComponentA greeting-message="Hello, Vue 3!" />
  <ComponentA :greeting-message="message" />
</template>
```

> 靜態 prop 是直接寫在模板中的，值是一個固定的字符串或數字。
> 動態 prop 的值是一個表達式，可以是 data 中的數據，也可以是 computed 或 method 的返回值。

## 傳遞不同類型的 prop

### Number

```template
<!-- 雖然 `42` 是個常量，我們還是需要使用 v-bind -->
<!-- 因為這是一個 JavaScript 表達式而不是一個字符串 -->
<BlogPost :likes="42" />

<!-- 根據一個變量的值動態傳入 -->
<BlogPost :likes="post.likes" />
```

### Boolean

```template
<!-- 僅寫上 prop 但不傳值，會隱式轉換為 `true` -->
<BlogPost is-published />

<!-- 雖然 `false` 是靜態的值，我們還是需要使用 v-bind -->
<!-- 因為這是一個 JavaScript 表達式而不是一個字符串 -->
<BlogPost :is-published="false" />

<!-- 根據一個變量的值動態傳入 -->
<BlogPost :is-published="post.isPublished" />
```

### Array

```template
<!-- 雖然這個數組是個常量，我們還是需要使用 v-bind -->
<!-- 因為這是一個 JavaScript 表達式而不是一個字符串 -->
<BlogPost :comment-ids="[234, 266, 273]" />

<!-- 根據一個變量的值動態傳入 -->
<BlogPost :comment-ids="post.commentIds" />
```

### Object

```template
<!-- 雖然這個對象字面量是個常量，我們還是需要使用 v-bind -->
<!-- 因為這是一個 JavaScript 表達式而不是一個字符串 -->
<BlogPost
  :author="{
    name: 'Veronica',
    company: 'Veridian Dynamics'
  }"
 />

<!-- 根據一個變量的值動態傳入 -->
<BlogPost :author="post.author" />
```

## 使用一個物件傳遞多個 prop

```js
const post = {
  id: 1,
  title: 'My Journey with Vue'
}
```

```template
<BlogPost v-bind="post" />
```

等於
  
```template
  <BlogPost :id="post.id" :title="post.title" />
```

## 單向數據流

```vue
const props = defineProps(['foo'])

// ❌ 警告！prop 是只讀的！
props.foo = 'bar'
```

> 所有 prop 都是單向綁定的：當父組件的屬性變化時，將傳遞給子組件，但是子組件不能直接修改父組件的屬性。
> 若要變化 prop 請使用 emit 事件，讓父組件去修改傳入的資料。

### 若是需要修改 prop 的值，可以使用 data 來接收 prop 的值

```vue
<script setup>
import { defineProps } from 'vue'

const props = defineProps(['foo'])
const data = ref(props.foo)
</script>
```

### 或是使用 computed 來接收 prop 的值

```vue
<script setup>

import { defineProps } from 'vue'

const props = defineProps(['foo'])
const data = computed(() => props.foo)

</script>
```

## Boolean 類型轉換

為了更接近原生 boolean 特性的行為，boolean 會有以下特性：

```vue
<!-- 等同於傳入 :disabled="true" -->
<MyComponent disabled />

<!-- 等同於傳入 :disabled="false" -->
<MyComponent />
```

可定義多個 prop 來接收不同的值，但 string 在 boolean 前，會被視為 string。

```vue
// disabled 將被轉換為 true
defineProps({
  disabled: [Boolean, Number]
})
  
// disabled 將被轉換為 true
defineProps({
  disabled: [Boolean, String]
})
  
// disabled 將被轉換為 true
defineProps({
  disabled: [Number, Boolean]
})
  
// disabled 將被解析為空字符串 (disabled="")
// 規則不一樣，string 在 boolean 前，會被視為 string
defineProps({
  disabled: [String, Boolean]
})
```
