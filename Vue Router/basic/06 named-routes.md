# named routes

## 命名路由

```javascript
const routes = [
  {
    path: '/user/:username',
    name: 'user',
    component: User,
  },
]
```

```html
<router-link :to="{ name: 'user', params: { username: 'eduardo' }}">User</router-link>
```

```javascript
router.push({ name: 'user', params: { username: 'eduardo' } });
```

> 命名路由讓代碼更容易理解和維護。它們可以在組件中或是 `<router-link>` 中使用。