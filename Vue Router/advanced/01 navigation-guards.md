# 01 Navigation Guards

## Global Guards

```js
const router = createRouter({ ... })
router.beforeEach((to, from) => {
   if (
     // 檢查用戶是否已經驗證
     !isAuthenticated &&
     // ❗️ 確保要前往的路由不是 Login，否則會陷入無限迴圈
     to.name !== 'Login'
   ) {
     // 重定向到登錄頁面
     return { name: 'Login' }
   }
})
```

> to: 正要進入的路由對象
> from: 正要離開的路由對象

異步路由組件也可以使用 `beforeEach` 來執行路由守衛。

```js
router.beforeEach(async (to, from) => {
  // canUserAccess() 返回 `true` 或 `false`
  const canAccess = await canUserAccess(to)
  if (!canAccess) return '/login'
})
```

若報錯會進入 `router.onError` 鉤子。

```js
router.onError((error) => {
  console.error('An error occurred:', error)
})
```

第三個參數 `next` 是一個函數，用於控制路由的流程。

```js
router.beforeEach((to, from, next) => {
  if (to.name === 'Login') {
    // 如果要前往的路由是 Login，則直接放行
    next()
  } else {
    // 否則重定向到 Login
    next({ name: 'Login' })
  }
})
```

> `next` 經由 RFC 討論後，將會移除，但目前仍然可以使用。

## Global Resolve Guards

```js
router.beforeResolve(async to => {
  if (to.meta.requiresCamera) {
    try {
      await askForCameraPermission()
    } catch (error) {
      if (error instanceof NotAllowedError) {
        // 取消導航
        return false
      } else {
        // 重新拋出錯誤
        throw error
      }
    }
  }
})
```

> guard 執行順序：`beforeEach` -> `beforeEnter` ->`beforeResolve`

## Global After Hooks

```js
router.afterEach((to, from) => {
  sendToAnalytics(to.fullPath)
})
```

> `afterEach` 不接受 `next` 函數，因為它不會影響導航。
> 可用來發送分析數據、滾動到頁面頂部等。

## Per-Route Guards

```js
const routes = [
  {
    path: '/users/:id',
    component: UserDetails,
    beforeEnter: (to, from) => {
      // reject the navigation
      return false
    },
  },
]
```

> `beforeEnter` 會在路由進入之前執行。
> 不同的路由導入才會執行 `beforeEnter`，只改變路由的 params、query、hash 不會觸發。

```js
function removeQueryParams(to) {
  if (Object.keys(to.query).length)
    return { path: to.path, query: {}, hash: to.hash }
}

function removeHash(to) {
  if (to.hash) return { path: to.path, query: to.query, hash: '' }
}

const routes = [
  {
    path: '/users/:id',
    component: UserDetails,
    beforeEnter: [removeQueryParams, removeHash],
  },
  {
    path: '/about',
    component: UserDetails,
    beforeEnter: [removeQueryParams],
  },
]
```

> `beforeEnter` 可以是一個函數或一個函數陣列。
> 對於不同路由，但可以重複使用的函數，也可以將其放在一個單獨的文件中。

## Component Guards

```vue
<script setup>
import { ref, onBeforeRouteLeave } from 'vue';
const name = ref('');
onBeforeRouteUpdate(to, from) {
  name.value = to.params.name
}
onBeforeRouteLeave (to, from) {
  const answer = window.confirm('Do you really want to leave? you have unsaved changes!')
  if (!answer) return false
}
</script>
```

> `onBeforeRouteLeave` 會在離開路由之前執行。
> `onBeforeRouteUpdate` 會在路由更新之前執行。

## The Full Navigation Resolution Flow

- Navigation triggered.
- Call `beforeRouteLeave` guards in **deactivated components**.
- Call **global** `beforeEach` guards.
- Call `beforeRouteUpdate` guards in **reused components**.
- Call `beforeEnter` in **route configs**.
- Resolve async route components.
- Call `beforeRouteEnter` in **activated components**.
- Call **global** `beforeResolve` guards.
- Navigation is confirmed.
- Call **global** `afterEach` hooks.
- DOM updates triggered.
- Call callbacks passed to next in `beforeRouteEnter` guards with instantiated instances.

> `beforeRouteLeave` -> `beforeEach` -> `beforeRouteUpdate` -> `beforeEnter` -> `beforeRouteEnter` -> `beforeResolve` -> `afterEach`