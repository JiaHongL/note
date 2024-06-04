# 04 Composition API

## useRoute 和 useRouter

```vue
import { useRouter, useRoute } from 'vue-router'

export default {
  setup() {
    const router = useRouter()
    const route = useRoute()

    function pushWithQuery(query) {
      router.push({
        name: 'search',
        query: {
          ...route.query,
          ...query,
        },
      })
    }
  },
}
```

> useRoute 可取得當前路由的資訊，
> useRouter 可取得 router 實例。

## useRote

```vue
import { useRoute } from 'vue-router'
import { ref, watch } from 'vue'

export default {
  setup() {
    const route = useRoute()
    const userData = ref()

    watch(
      () => route.params.id,
      async newId => {
        userData.value = await fetchUser(newId)
      }
    )
  },
}
```

> 可監聽路由的參數變化。

## onBeforeRouteLeave 和 onBeforeRouteUpdate

```vue
import { onBeforeRouteLeave, onBeforeRouteUpdate } from 'vue-router'
import { ref } from 'vue'

export default {
  setup() {

    onBeforeRouteLeave((to, from) => {
      const answer = window.confirm(
        'Do you really want to leave? you have unsaved changes!'
      )
      if (!answer) return false
    })

    const userData = ref()

    onBeforeRouteUpdate(async (to, from) => {
      if (to.params.id !== from.params.id) {
        userData.value = await fetchUser(to.params.id)
      }
    })
  },
}
```

> onBeforeRouteLeave 可在離開路由前執行操作。
> onBeforeRouteUpdate 可在路由更新前執行操作。
> 可在這裡執行一些清理或更新資料的操作或是確認是否要離開。
> 可以在其他元件中使用，類似 useXXXNavigationGuards。

```vue
import { RouterLink, useLink } from 'vue-router'
import { computed } from 'vue'

export default {
  name: 'AppLink',

  props: {
    ...RouterLink.props,
    inactiveClass: String,
  },

  setup(props) {
    const {
      route,
      href,
      isActive,
      isExactActive,
      navigate
      } = useLink(props)

    const isExternalLink = computed(
      () => typeof props.to === 'string' && props.to.startsWith('http')
    )

    return { isExternalLink, href, navigate, isActive }
  },
}
```

> 可以再其他元件中使用 useXXXLink 來取得 RouterLink 的資訊。
