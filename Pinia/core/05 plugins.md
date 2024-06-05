# Plugins

- 為 store 添加新的屬性
- 定義 store 時增加新的選項
- 為 store 增加新的方法
- 包裝現有的方法
- 改變甚至取消 action
- 實現副作用，如本地存儲
- 僅應用插件於特定 store

## 為 store 添加新的屬性

```js
// plugins/myPlugin.ts
pinia.use(() => ({ hello: 'world' }))

// 也可以直接幫 store 添加屬性
pinia.use(({ store }) => {
  store.hello = 'world'
})
```

## content
  
```js
export function myPiniaPlugin(context) {
  context.pinia
  context.app 
  context.store
  context.options
  // ...
}
```

> context.pinia: Pinia 實例
> context.app: Vue 應用實例
> context.store: 當前 store 實例
> context.options: store 的選項

## store.$subscribe

```js
pinia.use(({ store }) => {
  const unsubscribe = store.$subscribe((mutation, state) => {
    console.log('Mutation:', mutation);
    console.log('New state:', state);
  });
})
```

- mutation: 包含變化的詳細信息。
  - type: 表示變化類型（通常是 'patch'）。
  - storeId: 變化發生的 store 的 ID。
  - events: 一個包含變化詳細信息的數組，通常包括 key 和 newValue。
- state: store 的新狀態。

## store.$onAction

```js
pinia.use(({ store }) => {
  const unsubscribe = store.$onAction(({ name, store, args, after, onError }) => {
    console.log(`Action ${name} was called with args:`, args);

    after((result) => {
      console.log(`Action ${name} finished with result:`, result);
    });

    onError((error) => {
      console.error(`Action ${name} failed with error:`, error);
    });
  });
})
```

- name: 被調用的 action 名稱。
- store: 調用 action 的 store 實例。
- args: 調用 action 時傳遞的參數。
- after: 一個函數，接受一個回調函數，當 action 成功完成後會被調用。
- onError: 一個函數，接受一個回調函數，當 action 發生錯誤時會被調用。

## 整個 plugin 範例

```js
// main.js
import { createApp } from 'vue';
import { createPinia } from 'pinia';
import App from './App.vue';

const app = createApp(App);
const pinia = createPinia();

pinia.use(({ store }) => {
  store.$subscribe((mutation, state) => {
    console.log('Mutation:', mutation);
    console.log('New state:', state);
  });

  store.$onAction(({ name, store, args, after, onError }) => {
    console.log(`Action ${name} was called with args:`, args);

    after((result) => {
      console.log(`Action ${name} finished with result:`, result);
    });

    onError((error) => {
      console.error(`Action ${name} failed with error:`, error);
    });
  });
});

app.use(pinia);
app.mount('#app');
```

## store.$state

```js
import { toRef, ref } from 'vue'

pinia.use(({ store }) => {
  // 為了正確地處理 SSR，我們需要確保不會覆寫任何一個
  // 現有的值
  if (!Object.prototype.hasOwnProperty.call(store.$state, 'hasError')) {
    // 在插件中定義 hasError，因此每個 store 都有各自的
    // hasError 狀態
    const hasError = ref(false)
    // 在 `$state` 上設定變數，允許它在 SSR 期間被序列化
    store.$state.hasError = hasError
  }
  // 我們需要將 ref 從 state 轉移到 store
  // 這樣的話，兩種方式：store.hasError 和 store.$state.hasError 都可以存取
  // 並且共享的是同一個變數
  // 查看 https://cn.vuejs.org/api/reactivity-utilities.html#toref
  store.hasError = toRef(store.$state, 'hasError')

  // 在這種情況下，最好不要返回 `hasError`
  // 因為它將被顯示在 devtools 的 `state` 部分
  // 如果我們返回它，devtools 會顯示兩次。
})
```

> 先在 state 定義 hasError，才在 store 上定義 hasError。
> 這個插件將為每個 store 添加一個 hasError 屬性。
> 需要在 state 上定義 hasError，這樣它才能在 SSR 時被序列化。
> 需要在 store 上定義 hasError，這樣它才能在 devtools 中被訪問。

## 增加外部屬性

```js
import { markRaw } from 'vue'
import { router } from './router'

pinia.use(({ store }) => {
  store.router = markRaw(router)
})
```

> 需要使用 markRaw 包裝一下。

## add new option

```js
defineStore('search', {
  actions: {
    searchContacts() {
      // ...
    },
  },

  // 會在後面被插件使用
  debounce: {
    // 讓 debounce 插件知道我們想要等待多久
    searchContacts: 300,
  },
})
```

```js
// plugins/debounce.ts
import debounce from 'lodash/debounce'

pinia.use(({ options, store }) => {
  if (options.debounce) {
    // we are overriding the actions with new ones
    return Object.keys(options.debounce).reduce((debouncedActions, action) => {
      debouncedActions[action] = debounce(
        store[action],
        options.debounce[action]
      )
      return debouncedActions
    }, {})
  }
})
```

> 這個插件會將 debounce 選項應用到 store 上的 action。
