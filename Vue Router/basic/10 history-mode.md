# 10 history mode

## Hash 模式

```js
import { createRouter, createWebHashHistory } from 'vue-router'

const router = createRouter({
  history: createWebHashHistory(),
  routes: [
    //...
  ],
})
```

> 這是預設的模式，URL 會帶有 `#`。

## Memory 模式

```js
import { createRouter, createMemoryHistory } from 'vue-router'
const router = createRouter({
  history: createMemoryHistory(),
  routes: [
    //...
  ],
})
```

> 這個模式不會改變 URL，並且不會被瀏覽器記錄。
> 這個模式通常用於測試或 SSR。

## Html5 模式

```js
import { createRouter, createWebHistory } from 'vue-router'

const router = createRouter({
  history: createWebHistory(),
  routes: [
    //...
  ],
})
```

## 伺服器配置

若是使用 HTML5 模式，你需要在伺服器上配置一個規則，讓所有的路由都指向你的 index.html 文件，因為 Vue Router 是一個單頁應用，所有的路由都應該指向同一個文件。

[參考官網](https://router.vuejs.org/zh/guide/essentials/history-mode.html#%E6%9C%8D%E5%8A%A1%E5%99%A8%E9%85%8D%E7%BD%AE%E7%A4%BA%E4%BE%8B)

