# dynamic-matching

## 設定 ID

```javascript
const routes = [
  { path: '/users/:id', component: User },
];
```

```javascript
const User = {
  template: '<div>User {{ $route.params.id }}</div>',
};
```

> 就可以從 `$route.params` 取得動態的參數。

## 也可以在一個路由中設定多個動態參數

| 匹配模式 | 匹配路径 | $route.params |
| --- | --- | --- |
| /users/:username | /users/eduardo | { username: 'eduardo' } |
| /users/:username/posts/:postId | /users/eduardo/posts/123 | { username: 'eduardo', postId: '123' } |

## 監聽路由變化

```vue
<template>
  <!-- template content -->
</template>

<script setup>
import { watch } from 'vue'
import { useRoute } from 'vue-router'

const route = useRoute()

watch(
  () => route.params,
  (toParams, previousParams) => {}
)
</script>
```

或是使用 beforeRouteUpdate

```javascript
const User = {
  template: '...',
  async beforeRouteUpdate(to, from) {
    // 对路由变化做出响应...
    this.userData = await fetchUser(to.params.id)
  },
};
```

## 404 路由設定

```javascript
const router = new VueRouter({
  routes: [
    { path: '/:pathMatch(.*)*', name: 'NotFound', component: NotFound },
  ],
});
```

### 配對到 user 開頭的路由

```javascript
const routes = [
  // 將配對以 `/user-` 開頭，並將後面的內容都 `$route.params.afterUser` 下
  { path: '/user-:afterUser(.*)', component: UserGeneric },
]
```

> 這樣就可以將 `/user-123` 的 `123` 放到 `$route.params.afterUser` 下。
> 這樣就可以將 `/user-123/456` 的 `123/456` 放到 `$route.params.afterUser` 下。

### 使用 router.push 導向 404 頁面

```javascript
this.$router.push({
  name: 'NotFound',
  // 保留當前路徑並刪除第一個字符，以避免目標 URL 以 `//` 開頭。
  params: { pathMatch: this.$route.path.substring(1).split('/') },
  // 保留現有的查詢和 hash 值，如果有的話
  query: this.$route.query,
  hash: this.$route.hash,
})
```
