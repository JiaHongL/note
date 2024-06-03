# 10 Watchers

## 基礎監聽

```javascript
const x = ref(0)
const y = ref(0)

// 單個 ref
watch(x, (newX) => {
  console.log(`x is ${newX}`)
})

// getter 函數
watch(
  () => x.value + y.value,
  (sum) => {
    console.log(`sum of x + y is: ${sum}`)
  }
)

// 多個 ref
watch([x, () => y.value], ([newX, newY]) => {
  console.log(`x is ${newX} and y is ${newY}`)
})

// 移除監聽
const stop = watch(x, (newX) => {
  console.log(`x is ${newX}`)
})

// 停止監聽
stop()
```

## 若是監聽 reactive 物件的屬性，則可以直接監聽物件本身，或是使用 getter 函數

```javascript
const obj = reactive({ count: 0 })

// 直接監聽物件
watch(obj, (newObj) => {
  console.log(`count is: ${newObj.count}`)
})

// getter 函數

watch(
  () => obj.count,
  (count) => {
    console.log(`count is: ${count}`)
  }
)
```

## deep 監聽

```javascript
const obj = reactive({
  nested: {
    count: 0
  }
})

watch(
  () => state.someObject,
  () => {
    // 僅當 state.someObject 被替換時觸發
  }
)

watch(
  () => state.someObject,
  (newValue, oldValue) => {
    // 注意：`newValue` 此處和 `oldValue` 是相等的
    // *除非* state.someObject 被整個替換了
  },
  { deep: true }
)
```

> 注意：deep 監聽會對整個物件進行深度監聽，因此會對物件的所有屬性進行監聽，這可能會導致性能問題。

## immediate 監聽

```javascript
const x = ref(0)

watch(
  x,
  (newX, oldX) => {
    console.log(`x is ${newX}, old x is ${oldX}`)
  },
  { immediate: true }
)
```

> 注意：immediate 監聽會在監聽設置完成後立即執行一次回調函數。
> 這對於初始化時需要立即執行的邏輯非常有用。

## once 監聽

```javascript
const x = ref(0)

watch(
  x,
  (newX, oldX) => {
    console.log(`x is ${newX}, old x is ${oldX}`)
  },
  { once: true }
)
```

> 注意：once 監聽會在第一次觸發後自動停止監聽。

## watchEffect

```javascript
const x = ref(0)

watchEffect(() => {
  console.log(`x is ${x.value}`)
});

// 停止監聽
const stop = watchEffect(() => {
  console.log(`x is ${x.value}`)
});

stop()
```

> 注意：watchEffect 會立即執行一次回調函數，並在依賴的 ref 發生變化時再次執行。
> 避免使用 watchEffect 來執行具有副作用的操作，例如異步請求或 DOM 操作。

## Callback Flush Timing 回調的觸發時機

### pre - 在資料更新後，DOM 更新之前執行。 (預設)

```javascript
watch(
  () => x.value,
  () => {
    console.log(`x is ${x.value}`)
  },
  { flush: 'pre' }
)
```

### post - 在 DOM 更新之後執行。

```javascript
watch(
  () => x.value,
  () => {
    console.log(`x is ${x.value}`)
  },
  { flush: 'post' }
)
```

### sync - 資料更新後立即執行。

```javascript
watch(
  () => x.value,
  () => {
    console.log(`x is ${x.value}`)
  },
  { flush: 'sync' }
)
```

> 注意：sync 回調會在依賴的 ref 發生變化時立即執行，這可能會導致性能問題。

### pre、post、sync 的比較

```vue
<template>
  <div>
    <p>{{ count }}</p>
    <button @click="increment">Increment</button>
  </div>
</template>

<script setup>
import { ref, watch, nextTick, onUpdated, watchEffect, onBeforeUpdate } from 'vue';

const count = ref(0);

const increment = () => {
  count.value++;
};

// Before DOM update log
onBeforeUpdate(() => {
  console.log('DOM is about to be updated');
});

// flush: 'pre' (default)
watch(count, (newVal) => {
  console.log(`Count changed (pre): ${newVal}`);
}, { flush: 'pre' });

// flush: 'post'
watch(count, (newVal) => {
  console.log(`Count changed (post): ${newVal}`);
}, { flush: 'post' });

// flush: 'sync'
watch(count, (newVal) => {
  console.log(`Count changed (sync): ${newVal}`);
  console.log('Executing immediately after data change');
}, { flush: 'sync' });

// DOM update log
onUpdated(() => {
  console.log('DOM updated');
});

// nextTick log
watchEffect(async () => {
  await nextTick();
  console.log(`Next Tick: ${count.value}`);
});
</script>
```

```js
Next Tick: 0 // 初始化 watchEffect 會執行一次

Count changed (sync): 1
Executing immediately after data change
Count changed (pre): 1
DOM is about to be updated
Count changed (post): 1
DOM updated

Count changed (sync): 2
Executing immediately after data change
Count changed (pre): 2
DOM is about to be updated
Count changed (post): 2
DOM updated
```

> Date Change == async update (immediate) > pre update > DOM updating > post update > DOM updated