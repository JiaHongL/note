# 05 navigation

## router-link

`<router-link>` 是一個可以讓用戶在應用中進行路由切換的元素。當點擊 `<router-link>` 時，目標路由的 URL 會在 `<router-view>` 中渲染。

```html
<router-link to="/foo">Go to Foo</router-link>
<!-- 帶 params -->
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
<!-- 帶 query -->
<router-link :to="{ path: 'register', query: { plan: 'private' }}">Register</router-link>
<!-- 帶 hash -->
<router-link :to="{ path: 'about', hash: '#team' }">About</router-link>
```

## rotuer.push

```javascript
// 使用字串路徑導向
router.push('/users/eduardo');

// 使用物件指定路徑
router.push({ path: '/users/eduardo' });

// 使用命名路由，並傳遞參數
router.push({ name: 'user', params: { username: 'eduardo' } });

// 使用查詢參數，網址將會是 /register?plan=private
router.push({ path: '/register', query: { plan: 'private' } });

// 使用 hash 值，網址將會是 /about#team
router.push({ path: '/about', hash: '#team' });
```

> `router.push` 可以接受與 `<router-link>` 相同的參數。

```javascript
const username = 'eduardo';

// 我们可以手動建立網址，但是我們必須自己處理編碼
router.push(`/user/${username}`); // -> /user/eduardo
// 相同
router.push({ path: `/user/${username}` }); // -> /user/eduardo
// 如果可以的話，使用 `name` 和 `params` 從自動 URL 編碼中獲益
router.push({ name: 'user', params: { username } }); // -> /user/eduardo
// `params` 不能與 `path` 一起使用
router.push({ path: '/user', params: { username } }); // -> /user
```

> path 不能與 params 同時使用，如果兩者都存在，params 會被忽略。

## router.replace

`router.replace` 與 `router.push` 類似，但是它不會留下歷史記錄。

```javascript
// 使用字串路徑導向
router.replace('/users/eduardo');

// 使用物件指定路徑
router.replace({ path: '/users/eduardo' });

// 使用命名路由，並傳遞參數
router.replace({ name: 'user', params: { username: 'eduardo' } });

// 使用查詢參數，網址將會是 /register?plan=private
router.replace({ path: '/register', query: { plan: 'private' } });

// 使用 hash 值，網址將會是 /about#team
router.replace({ path: '/about', hash: '#team' });
```

> `router.replace` 可以接受與 `<router-link>` 相同的參數。
