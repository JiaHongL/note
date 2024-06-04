# 03 Data Fetching

## Fetching After Navigation

```vue
<template>
  <div class="post">
    <div v-if="loading" class="loading">Loading...</div>

    <div v-if="error" class="error">{{ error }}</div>

    <div v-if="post" class="content">
      <h2>{{ post.title }}</h2>
      <p>{{ post.body }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'
import { useRoute } from 'vue-router'
import { getPost } from './api.js'

const route = useRoute()

const loading = ref(false)
const post = ref(null)
const error = ref(null)

// 當路由的 params 改變時，再次取得資料
watch(() => route.params.id, fetchData, { immediate: true })

async function fetchData(id) {
  error.value = post.value = null
  loading.value = true
  
  try {
    // 這裡的 `getPost` 是你的資料取得工具或 API 包裝器
    post.value = await getPost(id)  
  } catch (err) {
    error.value = err.toString()
  } finally {
    loading.value = false
  }
}
</script>
```

## Fetching Before Navigation

```vue
<template>
  <div>
    <div v-if="error">{{ error }}</div>
    <div v-else-if="post">{{ post }}</div>
    <div v-else>Loading...</div>
  </div>
</template>

<script setup>
import { ref, onBeforeRouteUpdate, onBeforeRouteEnter } from 'vue';
import { useRoute } from 'vue-router';
import { getPost } from './api'; // 假設這是您獲取數據的方法

const post = ref(null);
const error = ref(null);

const setPost = (data) => {
  post.value = data;
};

const setError = (err) => {
  error.value = err.toString();
};

const route = useRoute();

onBeforeRouteEnter(async (to, from, next) => {
  try {
    const data = await getPost(to.params.id);
    next(vm => vm.setPost(data));
  } catch (err) {
    next(vm => vm.setError(err));
  }
});

onBeforeRouteUpdate((to, from, next) => {
  post.value = null;
  getPost(to.params.id).then(setPost).catch(setError);
  next();
});

const setPostCallback = (data) => {
  post.value = data;
};

const setErrorCallback = (err) => {
  error.value = err.toString();
};
</script>
```

> 進到這個頁面時，`onBeforeRouteEnter` 會在路由進入之前執行，會等取得資料後再進入頁面。`onBeforeRouteUpdate` 會在路由更新時執行，會等取得資料後再更新頁面。