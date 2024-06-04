# scroll behavior

## 基本用法

```js
const router = createRouter({
  history: createWebHashHistory(),
  routes: [...],
  scrollBehavior (to, from, savedPosition) {
    // return 期望滾動到哪個位置
  }
})
```

> `scrollBehavior` 函數接收 `to` 和 `from` 路由對象，以及 `savedPosition` 狀態，返回期望滾動到的位置。

## 返回值

```js
const router = createRouter({
  scrollBehavior(to, from, savedPosition) {
    // 始終滾動到頂部 
    return { top: 0 }
  },
})
```

> 始終滾動到頂部

## 指定滾動位置

```js
const router = createRouter({
  scrollBehavior(to, from, savedPosition) {
    // 始終在 #main 元素上 10 像素
    return {
      // 也可以這樣寫 
      // el: document.getElementById('main'),
      el: '#main',
      // 在元素上 10 像素
      top: 10,
    }
  },
})
```

> 始終在 #main 元素上 10 像素

## 恢復瀏覽器記錄的滾動位置

```js
const router = createRouter({
  scrollBehavior(to, from, savedPosition) {
    if (savedPosition) {
      return savedPosition
    } else {
      return { top: 0 }
    }
  },
})
```

> 如果有 `savedPosition` 狀態，則恢復到該位置，否則滾動到頂部。
> `savedPosition` 是瀏覽器自動管理的滾動位置。當用戶使用瀏覽器的前進和後退按鈕進行導航時，瀏覽器會自動保存每個頁面的滾動位置。這使得用戶在返回上一頁或前往下一頁時，可以自動回到之前滾動的位置。

## 苗點滾動

```js
const router = createRouter({
  scrollBehavior(to, from, savedPosition) {
    if (to.hash) {
      return {
        el: to.hash,
        behavior: 'smooth',
      }
    }
  }
})
```

> 如果目標路由有 `hash`，則滾動到該 `hash` 元素，並使用平滑滾動。

## 延遲滾動

```js
const router = createRouter({
  scrollBehavior(to, from, savedPosition) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve({ left: 0, top: 0 })
      }, 500)
    })
  },
})
```

> 延遲 500 毫秒後滾動到頂部

