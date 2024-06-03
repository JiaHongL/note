# List Rendering

## v-for

```js
const items = ref([{ message: 'Foo' }, { message: 'Bar' }])
```

```template
<li v-for="item in items">
  {{ item.message }}
</li>
```

## v-for with index

```template
<li v-for="(item, index) in items">
  {{ index }} - {{ item.message }}
</li>
```

## v-for with object

```js
const object = ref({
  title: 'How to do lists in Vue'
})
```

```template
<div>
  <div v-for="(value, key, index) in object">
    {{ index }} - {{ key }}: {{ value }}
  </div>
</div>
```

## v-for with range

```template
<div>
  <span v-for="n in 10">{{ n }} </span>
</div>
```

> 1 2 3 4 5 6 7 8 9 10
> 可減少渲染次數

## v-for with v-if

```template
<template v-for="todo in todos">
  <li v-if="!todo.isComplete">
    {{ todo.name }}
  </li>
</template>
```

> 建議包一層 template 來使用 v-for 和 v-if

## key attribute

```template
<div v-for="item in items" :key="item.id">
  {{ item.message }}
</div>
```

> 用 key 來幫助 Vue 來辨識每個元素，避免重複渲染 (reusing elements)

## v-for with v-model

```js
const todos = ref([
  { id: 1, name: 'Learn Vue', isComplete: false },
  { id: 2, name: 'Learn React', isComplete: false },
  { id: 3, name: 'Learn Angular', isComplete: false }
])
```

```template
<template v-for="todo in todos">
  <li>
    <input type="checkbox" v-model="todo.isComplete">
    {{ todo.name }}
  </li>
</template>
```

## v-for with component
  
```js
const items = ref([{ message: 'Foo' }, { message: 'Bar' }])
```

```template
<my-component
  v-for="item in items" 
  :item="item">
</my-component>
```

## mutation methods

```js
const items = ref([{ message: 'Foo' }, { message: 'Bar' }])
```

- push()
- pop()
- shift()
- unshift()
- splice()
- sort()
- reverse()

```template
<button @click="items.push({ message: 'Baz' })">Add</button>
<button @click="items.pop()">Remove</button>
<button @click="items.shift()">Remove</button>
<button @click="items.unshift({ message: 'Baz' })">Add</button>
<button @click="items.splice(1, 1)">Remove</button>
<button @click="items.splice(1, 0, { message: 'Baz' })">Add</button>
<button @click="items.sort()">Sort</button>
<button @click="items.reverse()">Reverse</button>
```

## filter

```js
const items = ref([{ message: 'Foo' }, { message: 'Bar' }]);
// `items` 是一個數組的 ref
items.value = items.value.filter((item) => item.message.match(/Foo/))
```

```template
<div>
  <div v-for="item in items" :key="item.id">
    {{ item.message }}
  </div>
</div>
```

## sort & reverse with computed

<!-- - return numbers.reverse() -->
<!-- + return [...numbers].reverse() -->

```js
const items = ref([{ message: 'Foo' }, { message: 'Bar' }])
const sortedItems = computed(() => [...items.value].sort())
const reversedItems = computed(() => [...items.value].reverse())
```

> 需注意 sort 和 reverse 會改變原始數組，所以要用 spread operator 來複製一份新的數組
