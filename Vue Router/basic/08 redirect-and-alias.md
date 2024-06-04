# redirect and alias

## Redirect

```js
const routes = [{ path: '/home', redirect: '/' }]
```

也可以轉向一個命名的路由：

```js
const routes = [{ path: '/home', redirect: { name: 'user' } }]
```

或是動態的轉換路由：

```js
const routes = [{
  // /search/screens -> /search?q=screens
  path: '/search/:searchText',
  redirect: to => {
    return { path: '/search', query: { q: to.params.searchText } }
  },
}];
```

> 注意：guard 只會作用在原始路由上，不會作用在 redirect 上。

## 相對路徑的 redirect

```js
const routes = [
  {
    // 將 /users/123/posts 重定向到 /users/123/profile 
    path: '/users/:id/posts',
    redirect: to => {
      // 相對路徑不以 `/` 開頭 或 { path: 'profile' }
      return 'profile'
    },
  },
]
```

## 別名 alias

```js
const routes = [
  { path: '/home', component: Home, alias: '/' }
]
```

> 別名 (alias) 會讓 `/home` 和 `/` 顯示相同的內容。

```js
const routes = [
  {
    path: '/users',
    component: UsersLayout,
    children: [
      // 這三個路由都會顯示 UserList component
      // - /users
      // - /users/list
      // - /people
      { path: '', component: UserList, alias: ['/people', 'list'] },
    ],
  },
]
```

```js
const routes = [
  {
    path: '/users/:id',
    component: UsersByIdLayout,
    children: [
      // 這三個路由都會顯示 UserDetails
      // - /users/24
      // - /users/24/profile
      // - /24
      { path: 'profile', component: UserDetails, alias: ['/:id', ''] },
    ],
  },
]
```
