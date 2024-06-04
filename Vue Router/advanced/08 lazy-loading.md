# 08 Lazy loading

```js
const UserDetails = () => import('./views/UserDetails.vue')

const router = createRouter({
  // ...
  routes: [
    { path: '/users/:id', component: UserDetails }
    // 或者在路由配置中直接使用
    { path: '/users/:id', component: () => import('./views/UserDetails.vue') },
  ],
})
```

> 使用 `import` 函數動態加載組件，實現組件的懶加載。
> 意思是到了這頁路由才會開始下載相關元件的 js 檔案，而不是一開始就全部下載。

## 元件按照群組加載

### 使用 Webpack

```js
const UserDetails = () => import(/* webpackChunkName: "group-user" */ './UserDetails.vue')
const UserDashboard = () => import(/* webpackChunkName: "group-user" */ './UserDashboard.vue')
const UserProfileEdit = () => import(/* webpackChunkName: "group-user" */ './UserProfileEdit.vue')
```

> 使用特殊的註釋 `webpackChunkName` 來指定元件屬於哪個群組。

### 使用 vite

```js
// vite.config.js
export default defineConfig({
  build: {
    rollupOptions: {
      // https://rollupjs.org/guide/en/#outputmanualchunks
      output: {
        manualChunks: {
          'group-user': [
            './src/UserDetails',
            './src/UserDashboard',
            './src/UserProfileEdit',
          ],
        },
      },
    },
  },
})
```

> 使用 `rollupOptions` 的 `manualChunks` 選項來指定元件屬於哪個群組。
