# meta

可以將任意訊息加到路由物件中，這樣就可以在路由守衛中使用這些訊息。

```js
const routes = [
  {
    path: '/posts',
    component: PostsLayout,
    children: [
      {
        path: 'new',
        component: PostsNew,
        // 只有登入的使用者才能新增文章
        meta: { requiresAuth: true },
      },
      {
        path: ':id',
        component: PostsDetail
        // 不需要登入就能查看文章
        meta: { requiresAuth: false },
      }
    ]
  }
]
```

在路由守衛中可以透過 `to.meta` 取得這些訊息。

```js
router.beforeEach((to, from, next) => {
  if (to.meta.requiresAuth && !isLoggedIn()) {
    next({ name: 'Login' })
  } else {
    next()
  }
})
```
