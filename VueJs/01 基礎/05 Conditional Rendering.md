# Conditional Rendering

## v-if

```html
<h1 v-if="awesome">Vue is awesome!</h1>
```

## v-else

```html
<h1 v-if="awesome">Vue is awesome!</h1>
<h1 v-else>Oh no 😢</h1>
```

## v-else-if

```html
<h1 v-if="type === 'A'">A</h1>
<h1 v-else-if="type === 'B'">B</h1>
<h1 v-else-if="type === 'C'">C</h1>
<h1 v-else>Not A/B/C</h1>
```

> v-else-if、v-else 和 v-if 必須是連續元素。

## v-template

```html
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

> v-template 不會被渲染到 DOM 中。

## v-show

```html
<h1 v-show="ok">Hello!</h1>
```

> v-show 只是簡單地切換元素的 CSS display 屬性。

## v-if vs v-show

- v-if：真實的條件渲染，切換時會銷毀或重建元素。(適合在切換頻率較低的情況下使用)
- v-show：簡單的切換 CSS display 屬性。(適合在切換頻率較高的情況下使用)

## v-if with v-for

```html
<ul>
  <li v-for="item in items" v-if="item.isActive">
    {{ item.name }}
  </li>
</ul>
```
> 不推薦在同一個元素上同時使用 v-if 和 v-for。如果需要這樣做，請將 v-if 放在容器元素上。