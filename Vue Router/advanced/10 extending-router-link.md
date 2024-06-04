# 10 extending-router-link

可以客製化 `RouterLink` 元件，讓它支援更多功能。

```vue
<template>
  <a
    :href="href"
    :class="[{ active: isActive, exact: isExactActive }, inactiveClass]"
    @click="navigate"
  >
    <slot></slot>
  </a>
</template>

<script>
import { RouterLink, useLink } from 'vue-router'

export default {
  name: 'AppLink',

  props: {
    ...RouterLink.props,
    inactiveClass: String,
  },

  setup(props) {
    // `props` 包含 `to` 和任何其他可以傳遞給 <router-link> 的 prop
    const { navigate, href, route, isActive, isExactActive } = useLink(props)
    return { navigate, href, isActive, isExactActive, inactiveClass }
  },
}
</script>

<style scoped>
.active {
  font-weight: bold;
}
.exact {
  text-decoration: underline;
}
</style>

```

這個元件接受 `to` 和 `inactiveClass` 這兩個 prop，並且使用 `useLink` 來處理路由相關的邏輯。

就可以在其他地方使用這個元件：

```vue
<template>
  <AppLink to="/about" inactiveClass="inactive">
    About
  </AppLink>
</template>

<script setup>
import AppLink from './AppLink.vue'
</script>
```
