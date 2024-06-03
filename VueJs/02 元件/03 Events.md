# Events

## 觸發與監聽

```vue
<!-- MyComponent -->
<script setup>
const emits = defineEmits(['someEvent'])
</script>

<template>
  <button @click="$emit('someEvent')">click me</button>
</template>
```

```vue
<!-- ParentComponent -->
<MyComponent @someEvent="handleEvent" />
```

```vue
<!-- ParentComponent 可搭配修飾符使用 -->
<button @someEvent.stop="handleEvent">click me</button>
```

## 傳遞事件

### 傳遞參數

```vue
<!-- MyComponent -->
<script setup>
const emits = defineEmits(['someEvent'])
</script>

<template>
  <button @click="$emit('someEvent', 'hello')">click me</button>
</template>
```

```vue
<!-- ParentComponent -->
<script setup>
const handleEvent = (msg) => {
  console.log(msg) // hello
}
</script>
<MyComponent @someEvent="handleEvent" />
```

## 傳送多個參數

```vue
<!-- MyComponent -->

<script setup>

const emits = defineEmits(['someEvent'])

</script>

<template>
<button @click="$emit('someEvent', 'hello', 'world')">click me</button>
</template>
```

```vue
<!-- ParentComponent -->

<script setup>

const handleEvent = (msg1, msg2) => {
console.log(msg1) // hello
console.log(msg2) // world
}

</script>

<MyComponent @someEvent="handleEvent" />
```
