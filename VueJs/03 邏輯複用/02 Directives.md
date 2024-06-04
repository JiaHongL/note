# 01 Directives

## 在元件內使用

```vue
<script setup>
// 在模板中啟用 v-focus
const vFocus = {
  mounted: (el) => el.focus()
}
</script>

<template>
  <input v-focus />
</template>
```

> 只能在 `<script setup>` 中使用

## 未使用 setup

```vue
<script>
export default {
  directives: {
    focus: {
      mounted(el) {
        el.focus()
      }
    }
  }
}
</script>
```

> 其他元件中也可使用

## 全域註冊

```javascript
const app = createApp({})

// 使 v-focus 在所有組件中都可用
app.directive('focus', {
  /* ... */
})
```

```javascript

## Directive Hook

```javascript
const myDirective = {
  // 在綁定元素的 attribute 前
  // 或事件監聽器應用前調用
  created(el, binding, vnode, prevVnode) {
    // 下面會介紹各個參數的細節
  },
  // 在元素被插入到 DOM 前調用
  beforeMount(el, binding, vnode, prevVnode) {},
  // 在綁定元素的父組件
  // 及他自己的所有子節點都掛載完成後調用
  mounted(el, binding, vnode, prevVnode) {},
  // 綁定元素的父組件更新前調用
  beforeUpdate(el, binding, vnode, prevVnode) {},
  // 在綁定元素的父組件
  // 及他自己的所有子節點都更新後調用
  updated(el, binding, vnode, prevVnode) {},
  // 綁定元素的父組件卸載前調用
  beforeUnmount(el, binding, vnode, prevVnode) {},
  // 綁定元素的父組件卸載後調用
  unmounted(el, binding, vnode, prevVnode) {}
}
```

## Directive Arguments

- el：指令綁定到的元素。這可以用於直接操作 DOM。
- binding：一個對象，包含以下屬性。
  - value：傳遞給指令的值。例如在 v-my-directive="1 + 1" 中，值是 2。
  - oldValue：之前的值，僅在 beforeUpdate 和 updated 中可用。無論值是否更改，它都可用。
  - arg：傳遞給指令的參數 (如果有的話)。例如在 v-my-directive:foo 中，參數是 "foo"。
  - modifiers：一個包含修飾符的對象 (如果有的話)。例如在 v-my-directive.foo.bar 中，修飾符對象是 { foo: true, bar: true }。
  - instance：使用該指令的組件實例。
  - dir：指令的定義對象。
- vnode：代表綁定元素的底層 VNode。
- prevVnode：代表之前的渲染中指令所綁定元素的 VNode。僅在 beforeUpdate 和 updated 鉤子中可用。

```html
<div v-example:foo.bar="baz">
```

```javascript
{
  arg: 'foo',
  modifiers: { bar: true },
  value: /* value of `baz` */,
  oldValue: /* value of `baz` from previous update */
}
```

動態參數
  
```html
<div v-example:[arg]="value">
```

## Function Shorthand

```html
<div v-color="color"></div>
```

```javascript
app.directive('color', (el, binding) => {
  // 這會在 `mounted` 和 `updated` 時都調用
  el.style.color = binding.value
})
```

> 只有 `mounted` 和 `updated` 會被調用

## Object Literals

```html
<div v-demo="{ color: 'white', text: 'hello!' }"></div>
```

```javascript
app.directive('demo', (el, binding) => {
  console.log(binding.value.color) // => "white"
  console.log(binding.value.text) // => "hello!"
})
```

> 可以使用物件字面量傳遞多個值


## 自元件上使用

```vue
<MyComponent v-demo="test" />
```

```html
<!-- MyComponent 的模板 -->
<div> <!-- v-demo 指令會被應用在此處 -->
  <span>My component content</span>
</div>
```

> 指令會被應用在元素的最外層
> 遇到多個根元素是無法使用，也無法使用 $attrs
> 不建議直接使用在元件上
