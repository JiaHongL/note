# Computed

## 1. 基本用法

```vue
<template>
  <div>
    <p>{{ message }}</p>
    <p>{{ reversedMessage }}</p>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const message = ref('Hello')
const reversedMessage = computed(() => {
  return message.value.split('').reverse().join('')
});
</script>
```

## 2.Writable Computed (Getter 與 Setter)

```vue
<script setup>
import { ref, computed } from 'vue'

const firstName = ref('John')
const lastName = ref('Doe')

const fullName = computed({
  // getter
  get() {
    return firstName.value + ' ' + lastName.value
  },
  // setter
  set(newValue) {
    // 注意：我們這裡使用的是解構賦值語法
    [firstName.value, lastName.value] = newValue.split(' ')
  }
})
</script>
```

