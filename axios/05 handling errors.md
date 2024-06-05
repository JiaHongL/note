# 05 handing errors

```js
axios.get('/user/12345')
  .catch(function (error) {
    if (error.response) {
      // 請求已成功發出且伺服器也響應了狀態碼，但狀態碼超出了 2xx 的範圍
      console.log(error.response.data);
      console.log(error.response.status);
      console.log(error.response.headers);
    } else if (error.request) {
      // 請求已成功發起，但沒有收到回應
      // `error.request` 在瀏覽器中是 XMLHttpRequest 的實例，
      // 而在 node.js 中是 http.ClientRequest 的實例
      console.log(error.request);
    } else {
      // 發送請求時出了點問題
      console.log('Error', error.message);
    }
    console.log(error.config);
  });
```

```js
axios.get('/user/12345', {
  validateStatus: function (status) {
    return status < 500; // 處理狀態碼小於 500 的情況
  }
});
```

> 你可以使用 `validateStatus` 選項來定義一個自訂的 HTTP 狀態碼的錯誤範圍。

```js
axios.get('/user/12345')
  .catch(function (error) {
    console.log(error.toJSON());
  });
``` 

> `error.toJSON()` 會返回一個包含所有錯誤訊息的物件。