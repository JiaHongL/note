# 06 Transitions

## Transitioning Between Routes

```html
<router-view v-slot="{ Component }">
  <transition name="fade">
    <component :is="Component" />
  </transition>
</router-view>
```

> 需要使用 slot 搭配 transition 添加過場動畫。

## 每個路由不同過場動畫

```js
const routes = [
  {
    path: '/custom-transition',
    component: PanelLeft,
    meta: { transition: 'slide-left' },
  },
  {
    path: '/other-transition',
    component: PanelRight,
    meta: { transition: 'slide-right' },
  },
]
```

```html
<router-view v-slot="{ Component, route }">
  <!-- 使用任何自定义过渡和回退到 `fade` -->
  <transition :name="route.meta.transition || 'fade'">
    <component :is="Component" />
  </transition>
</router-view>
```

> 透過 `route.meta.transition` 取得自定義過場動畫。

## 基於路由的動態轉換

```html
<!-- use a dynamic transition name -->
<router-view v-slot="{ Component, route }">
  <transition :name="route.meta.transition">
    <component :is="Component" />
  </transition>
</router-view>
```

```js
router.afterEach((to, from) => {
  const toDepth = to.path.split('/').length
  const fromDepth = from.path.split('/').length
  to.meta.transition = toDepth < fromDepth ? 'slide-right' : 'slide-left'
})
```

> 透過 `router.afterEach` 動態設定過場動畫。

## 強制重新過場動畫

```html
<router-view v-slot="{ Component, route }">
  <transition name="fade">
    <component :is="Component" :key="route.path" />
  </transition>
</router-view>
```

> 透過 `:key` 強制重新過場動畫。