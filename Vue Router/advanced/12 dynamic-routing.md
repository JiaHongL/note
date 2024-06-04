# 12 Dynamic Routing

## 新增路由

```js
const router = createRouter({
  history: createWebHistory(),
  routes: [{ path: '/:articleName', component: Article }],
})
```


```js
router.addRoute({ path: '/about', component: About })
router.replace(router.currentRoute.value.fullPath)
```

> 這樣就可以動態新增路由了，不過要注意的是，新增的路由會在最後面，所以如果有相同路徑的路由，會以最後新增的為主。
> 使用 `replace` 可以強制刷新畫面，讓新增的路由生效。

## 在 guard 中新增或刪除路由

```js
router.beforeEach(to => {
  if (!hasNecessaryRoute(to)) {
    router.addRoute(generateRoute(to))
    return to.fullPath
  }
})
```

> 在 `beforeEach` 中可以動態新增路由，不過要注意的是，新增的路由會在最後面，所以如果有相同路徑的路由，會以最後新增的為主。

## 刪除路由

### 增加一個 name 一樣的路由，就會刪除原本的路由

```js
router.addRoute({ path: '/about', name: 'about', component: About })
router.addRoute({ path: '/other', name: 'about', component: Other })
```

> 這樣就會刪除原本的路由，因為 Vue Router 會以 name 來判斷是否為同一個路由。

### 透過 removeRoute 刪除路由

```js
const removeRoute = router.addRoute(routeRecord)
removeRoute() // 刪除路由
```

### 使用 router.removeRoute() 刪除路由

```js
const route = router.addRoute({ path: '/about', component: About })
router.removeRoute('about')
```

## 加增 nested routes

```js
router.addRoute({ name: 'admin', path: '/admin', component: Admin })
router.addRoute('admin', { path: 'settings', component: AdminSettings })
```

> 這樣就可以新增巢狀路由了。

等同於

```js
router.addRoute({
  name: 'admin',
  path: '/admin',
  component: Admin,
  children: [{ path: 'settings', component: AdminSettings }],
})
```