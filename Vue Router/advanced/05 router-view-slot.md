# 05 Router View Slot

## RouterView Slot

```html
<router-view v-slot="{ Component }">
  <component :is="Component" />
</router-view>
```

> 透過 `v-slot` 取得 `Component`，並透過 `component` 動態渲染。

## KeepAlive & Transition

```html
<router-view v-slot="{ Component }">
  <keep-alive>
    <component :is="Component" />
  </keep-alive>
</router-view>
```

> 透過 `keep-alive` 保持組件狀態。

```html
<router-view v-slot="{ Component }">
  <transition>
    <component :is="Component" />
  </transition>
</router-view>
```

> 透過 `transition` 添加過場動畫。

```html
<router-view v-slot="{ Component }">
  <transition>
    <keep-alive>
      <component :is="Component" />
    </keep-alive>
  </transition>
</router-view>
```

> 同時使用 `keep-alive` 和 `transition`。


## passing props and slots

```html
<router-view v-slot="{ Component }">
  <component :is="Component" some-prop="a value">
    <p>Some slotted content</p>
  </component>
</router-view>
```

> 透過 `component` 動態渲染，並傳遞 `props` 和 `slots`。
