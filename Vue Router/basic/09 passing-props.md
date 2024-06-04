# 09 passing-props

```javascript
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}
const routes = [{ path: '/user/:id', component: User }]
```

換成

```javascript
const User = {
  props: ['id'],
  template: '<div>User {{ id }}</div>'
}
const routes = [{ path: '/user/:id', component: User, props: true }]
```

> 這樣就可以直接在 User 組件中使用 `id` 這個 prop 了。
> 當 props 被設定為 true 時，route.params 將會被設定為 props。

## 若有命名視圖，就必須幫每個命名視圖設定一個 props

```javascript
const routes = [
  {
    path: '/user/:id',
    components: { 
      default: User, 
      sidebar: Sidebar 
    },
    props: { 
      default: true, 
      sidebar: false 
    }
  }
]
```

## Object 模式

```javascript
const routes = [
  { path: '/promotion/from-newsletter', component: Promotion, props: { newsletterPopup: false } }
]
```

> 這樣就可以直接在 Promotion 組件中使用 `newsletterPopup` 這個 prop 了。

## Function 模式

```javascript
const routes = [
  { path: '/search', component: Search, props: (route) => ({ query: route.query.q }) }
]
```

> 這樣就可以直接在 Search 組件中使用 `query` 這個 prop 了。

## 透過 slot 傳遞 props

```javascript
<RouterView v-slot="{ Component }">
  <component
    :is="Component"
    view-prop="value"
   />
</RouterView>
```

> 這樣就可以直接在 Component 組件中使用 `view-prop` 這個 prop 了，但是每個元件都會接收到這個 prop，但不勢必要的。所以盡量用前面的方法。