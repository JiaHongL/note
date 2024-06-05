# 06 cancellation

## AbortController

```js
const controller = new AbortController();

axios.get('/foo/bar', {
   signal: controller.signal
}).then(function(response) {
   //...
});

// 取消請求
controller.abort()
```