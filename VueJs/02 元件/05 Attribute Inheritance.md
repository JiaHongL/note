# 05 Attribute Inheritance

## 屬性繼承

```vue
<!-- MyButton -->
<button>click me</button>
```

```vue
<!-- Parent -->
<MyButton class="large" />
```

```html
<!-- Rendered -->
<button class="large">click me</button>
```

> 這裡的 `class` 是繼承自 `MyButton` 的。

## class 與 style 繼承合併

```vue
<!-- MyButton -->
<button class="btn">click me</button>
```

```vue
<!-- Parent -->
<MyButton class="large" />
```

```html
<!-- Rendered -->
<button class="btn large">click me</button>
```

## v-on 繼承

```vue
<MyButton @click="onClick" />
```

```html
<button @click="onClick">click me</button>
```

> 也會繼承 `v-on` 到子元件的根元素。

## Nested Component Inheritance

```vue
<!-- <MyButton/> 的模板，只是渲染另一個組件 -->
<BaseButton />
```

```vue
<!-- Parent -->
<MyButton class="large" />
```

```html
<!-- Rendered -->
<button class="btn large">click me</button>
```

> `BaseButton` 也會繼承 `MyButton` 的 `class`。
> 若是 MyButton 有宣告過的 props 或 event，不會繼承到 BaseButton。

## 禁用繼承

```vue
<script setup>
defineOptions({
  inheritAttrs: false
})
// ...setup 邏輯
</script>
```

> 3.3 開始支援

可使用 $attrs 取得所有繼承的屬性。

```vue
<script setup>
const { $attrs } = useAttrs()
</script>

<template>
  <span>Fallthrough attribute: {{ $attrs }}</span>
</template>
```

> $attrs 會回傳一個物件，除了宣告的 props 和 event，還會其他屬性繼承，例如：class，style，v-on 監聽器等等。
> 和 props 不同的是，$attrs 保留了原始的屬性名稱，而不是轉換成 camelCase。例如：`foo-bar` 會保留為 `foo-bar`。

```vue
<div class="btn-wrapper">
  <button class="btn" v-bind="$attrs">click me</button>
</div>
```

> 這樣就可以將所有繼承的屬性綁定到子元素指定的元素上。
