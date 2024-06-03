# 04 Component v-model

## v-model

`v-model` 是一個自定義事件的縮寫，用於在組件上創建雙向數據綁定。

> vue 3.4.0 新增

```vue
<!-- Child.vue -->
<script setup>
const model = defineModel()

function update() {
  model.value++
}
</script>

<template>
  <div>Parent bound v-model is: {{ model }}</div>
</template>
```

```vue
<!-- Parent.vue -->
<Child v-model="countModel" />
```

## 運作原理

`v-model` 會自動將 `modelValue` 和 `update:modelValue` 作為 props 傳遞給子組件。

```vue
<!-- Child.vue -->
<script setup>
const props = defineProps(['modelValue'])
const emit = defineEmits(['update:modelValue'])
</script>

<template>
  <input
    :value="props.modelValue"
    @input="emit('update:modelValue', $event.target.value)"
  />
</template>
```

```html
<!-- Parent.vue -->
<Child
  :modelValue="foo"
  @update:modelValue="$event => (foo = $event)"
/>
```

## 設定必填與預設值

```vue
<!-- // 使 v-model 必填 -->
const model = defineModel({ required: true })

<!-- // 提供一個默認值 -->
const model = defineModel({ default: 0 })
```

## v-model 接受參數

```vue
<script setup>
const firstName = defineModel('firstName')
const lastName = defineModel('lastName')
</script>

<template>
  <input type="text" v-model="firstName" />
  <input type="text" v-model="lastName" />
</template>
```

```vue
<!-- Parent.vue -->
<Child 
  v-model:first-name="firstName" 
  v-model:last-name="lastName" 
/>
```

## v-model 修飾符

```vue
<!-- Child.vue -->
<script setup>
const [model, modifiers] = defineModel({
  set(value) {
    if (modifiers.capitalize) {
      return value.charAt(0).toUpperCase() + value.slice(1)
    }
    return value
  }
})
</script>

<template>
  <input type="text" v-model="model" />
</template>

<!-- Parent.vue -->
<Child v-model.capitalize="myText" />
```

> 可以使用 get 和 set 來自定義 v-model 的行為。

## 多個 v-model 參數的修飾符

```vue
<UserName
  v-model:first-name.capitalize="first"
  v-model:last-name.uppercase="last"
/>

<script setup>
// 定義第一個 model 和對應的修飾符
const [firstName, firstNameModifiers] = defineModel('firstName', {
  set(value) {
    if (firstNameModifiers.capitalize) {
      return value.charAt(0).toUpperCase() + value.slice(1);
    }
    return value;
  }
});

// 定義第二個 model 和對應的修飾符
const [lastName, lastNameModifiers] = defineModel('lastName', {
  set(value) {
    if (lastNameModifiers.uppercase) {
      return value.toUpperCase();
    }
    return value;
  }
});
</script>
```

vue 3.4.0 之前的版本，可以使用以下方法：
```vue
<script setup>
const props = defineProps({
firstName: String,
lastName: String,
firstNameModifiers: { default: () => ({}) },
lastNameModifiers: { default: () => ({}) }
})
defineEmits(['update:firstName', 'update:lastName'])

console.log(props.firstNameModifiers) // { capitalize: true }
console.log(props.lastNameModifiers) // { uppercase: true }
</script>
```