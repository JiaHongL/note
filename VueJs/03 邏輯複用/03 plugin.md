# 03 Plugin

下面是如何安裝一個插件的示範

```javascript
import { createApp } from 'vue'

const app = createApp({})

app.use(myPlugin, {
  /* 可選的選項 */
})
```

```javascript
const myPlugin = {
  install(app, options) {
    // 配置此應用
  }
}
```

> 插件是一個具有 install 方法的物件或函數，它可以添加全局功能。當 app.use(myPlugin) 被調用時，install 方法將被調用並且 app 實例將被傳遞過去。

插件沒有嚴格限定範圍，主要場景有：

- 通過 app.component() 和 app.directive() 註冊一到多個全局組件或自定義指令。
- 通過 app.provide() 使一個資源可被注入進整個應用。
- 向 app.config.globalProperties 中添加一些全局實例屬性或方法。

## 編寫插件

```javascript
// plugins/i18n.js
export default {
  install: (app, options) => {
    // 在這裡編寫插件代碼
  }
}
```

希望功能如下：

```html
<h1>{{ $translate('greetings.hello') }}</h1>
```


這個函數應當能夠在任意模板中被全局調用。這一點可以通過在插件中將它添加到 app.config.globalProperties 上來實現：

```javascript
// plugins/i18n.js
export default {
  install: (app, options) => {
    // 注入一個全局可用的 $translate() 方法
    app.config.globalProperties.$translate = (key) => {
      // 獲取 `options` 對象的深層屬性
      // 使用 `key` 作為索引
      return key.split('.').reduce((o, i) => {
        if (o) return o[i]
      }, options)
    }
  }
}
```

注入後，我們可以在任意模板中使用 $translate() 方法：

```javascript
import i18nPlugin from './plugins/i18n'

app.use(i18nPlugin, {
  greetings: {
    hello: 'Bonjour!'
  }
})
```

> $translate('greetings.hello') 就會在運行時被替換為 Bonjour!。

## 插件中的 Provide / Inject

插件也可以使用 provide 和 inject 來注入一個全局資源：

```js
// plugins/i18n.js
export default {
  install: (app, options) => {
    app.provide('i18n', options)
  }
}
```

之後就可以在任意模板中使用 inject 來訪問這個資源：

```vue
<script setup>
import { inject } from 'vue'

const i18n = inject('i18n')

console.log(i18n.greetings.hello)
</script>
```
