# 03 route-matching-syntax

## 自訂正則表達式的配對模式

```javascript
const routes = [
  // 配對 /o/3549
  { path: '/o/:orderId' },
  // 配對 /p/books
  { path: '/p/:productName' },
]
```

```javascript
const routes = [
  // /:orderId -> 配對數字
  { path: '/:orderId(\\d+)' },
  // /:productName -> 配對其他內容
  { path: '/:productName' },
]
```

> 例如 `/o/3549` 會配對到第一個路由，而 `/p/books` 會配對到第二個路由。
> 例如 `/3549` 會配對到第一個路由，而 `/books` 會配對到第二個路由。

## 配對多個路由

```javascript
const routes = [
  // /:chapters -> 配對 /one, /one/two, /one/two/three, 等
  { path: '/:chapters+' },
  // /:chapters -> 配對 /, /one, /one/two, /one/two/three, 等
  { path: '/:chapters*' },
]
```

> 例如 `/one/two/three` 會配對到第一個路由，而 `/one/two/three` 會配對到第二個路由。
> 例如 `/one/two/three` 會配對到第一個路由，而 `/` 會配對到第二個路由。

## Sensitive 与 strict 路由配置

默認情況下，路由是大小寫不敏感的，並且會忽略尾部的斜線。

```javascript
const router = createRouter({
  history: createWebHistory(),
  routes: [
    // 將匹配 /users/posva 而非：
    // - /users/posva/，當 strict: true
    // - /Users/posva，當 sensitive: true
    { path: '/users/:id', sensitive: true },
    // 將匹配 /users, /Users, 以及 /users/42 而非 /users/ 或 /users/42/
    { path: '/users/:id?' },
  ],
  strict: true, // 套用到所有路由
})
```

> strict: true，會檢查尾部的斜線。
> sensitive: true，會檢查大小寫。

## 可選路由參數

```javascript
const routes = [
  // 配對到 -> /users 和 /users/posva
  { path: '/users/:userId?' },
  // 配對到 -> /users 和 /users/42
  { path: '/users/:userId(\\d+)?' }
]
```

> \\d+ 代表數字。，? 代表可選。
