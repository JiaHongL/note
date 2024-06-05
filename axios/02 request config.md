# Request Config

```js
{
  // `url` 是用來請求的伺服器 URL
  url: '/user',

  // `method` 是建立請求時使用的方法
  method: 'get', // 預設值

  // `baseURL` 會自動加在 `url` 前面，除非 `url` 是一個絕對 URL。
  // 透過設定 `baseURL` 可以方便地為 axios 實例的方法傳遞相對 URL
  baseURL: 'https://some-domain.com/api/',

  // `transformRequest` 允許在向伺服器發送前修改請求資料
  // 它只能用於 'PUT', 'POST' 和 'PATCH' 這幾個請求方法
  // 陣列中最後一個函式必須返回一個字串、一個 Buffer 實例、ArrayBuffer、FormData，或 Stream
  // 你可以修改請求頭。
  transformRequest: [function (data, headers) {
    // 對發送的 data 進行任意轉換處理

    return data;
  }],

  // `transformResponse` 在傳遞給 then/catch 前，允許修改回應資料
  transformResponse: [function (data) {
    // 對接收的 data 進行任意轉換處理

    return data;
  }],

  // 自訂請求頭
  headers: {'X-Requested-With': 'XMLHttpRequest'},

  // `params` 是與請求一起發送的 URL 參數
  // 必須是一個簡單物件或 URLSearchParams 物件
  params: {
    ID: 12345
  },

  // `paramsSerializer` 是可選方法，主要用於序列化 `params`
  // (例如：https://www.npmjs.com/package/qs, http://api.jquery.com/jquery.param/)
  paramsSerializer: function (params) {
    return Qs.stringify(params, {arrayFormat: 'brackets'});
  },

  // `data` 是作為請求主體被發送的資料
  // 僅適用於 'PUT', 'POST', 'DELETE' 和 'PATCH' 請求方法
  // 在沒有設定 `transformRequest` 時，必須是以下類型之一：
  // - string, plain object, ArrayBuffer, ArrayBufferView, URLSearchParams
  // - 瀏覽器專屬: FormData, File, Blob
  // - Node 專屬: Stream, Buffer
  data: {
    firstName: 'Fred'
  },
  
  // 發送請求主體資料的可選語法
  // 請求方式 post
  // 只有 value 會被發送，key 則不會
  data: 'Country=Brasil&City=Belo Horizonte',

  // `timeout` 指定請求超時的毫秒數。
  // 如果請求時間超過 `timeout` 的值，則請求會被中斷
  timeout: 1000, // 預設值是 `0` (永不超時)

  // `withCredentials` 表示跨域請求時是否需要使用憑證
  withCredentials: false, // 預設值

  // `adapter` 允許自訂處理請求，這使測試更加容易。
  // 返回一個 promise 並提供一個有效的回應（參見 lib/adapters/README.md）。
  adapter: function (config) {
    /* ... */
  },

  // `auth` HTTP 基本認證
  auth: {
    username: 'janedoe',
    password: 's00pers3cret'
  },

  // `responseType` 表示瀏覽器將接收的回應資料類型
  // 選項包括：'arraybuffer', 'document', 'json', 'text', 'stream'
  // 瀏覽器專屬：'blob'
  responseType: 'json', // 預設值

  // `responseEncoding` 表示用於解碼回應的編碼 (Node.js 專屬)
  // 注意：忽略 `responseType` 的值為 'stream'，或者是用戶端請求
  responseEncoding: 'utf8', // 預設值

  // `xsrfCookieName` 是 xsrf token 的值，被用作 cookie 的名稱
  xsrfCookieName: 'XSRF-TOKEN', // 預設值

  // `xsrfHeaderName` 是帶有 xsrf token 值的 http 請求頭名稱
  xsrfHeaderName: 'X-XSRF-TOKEN', // 預設值

  // `onUploadProgress` 允許為上傳處理進度事件
  // 瀏覽器專屬
  onUploadProgress: function (progressEvent) {
    // 處理原生進度事件
  },

  // `onDownloadProgress` 允許為下載處理進度事件
  // 瀏覽器專屬
  onDownloadProgress: function (progressEvent) {
    // 處理原生進度事件
  },

  // `maxContentLength` 定義了 node.js 中允許的 HTTP 回應內容的最大位元組數
  maxContentLength: 2000,

  // `maxBodyLength`（僅 Node）定義允許的 HTTP 請求內容的最大位元組數
  maxBodyLength: 2000,

  // `validateStatus` 定義了對於給定的 HTTP 狀態碼是 resolve 還是 reject promise。
  // 如果 `validateStatus` 返回 `true`（或者設置為 `null` 或 `undefined`），
  // 則 promise 將會 resolved，否則是 rejected。
  validateStatus: function (status) {
    return status >= 200 && status < 300; // 預設值
  },

  // `maxRedirects` 定義了在 node.js 中要遵循的最大重定向數。
  // 如果設置為 0，則不會進行重定向
  maxRedirects: 5, // 預設值

  // `socketPath` 定義了在 node.js 中使用的 UNIX 套接字。
  // 例如：'/var/run/docker.sock' 來發送請求到 docker 守護進程。
  // 只能指定 `socketPath` 或 `proxy` 。
  // 如果都指定，則使用 `socketPath` 。
  socketPath: null, // 預設值

  // `httpAgent` 和 `httpsAgent` 定義了在 node.js 中進行 http 和 https 請求時使用的自訂代理。
  // 這允許添加如 `keepAlive` 等未啟用的預設選項。
  httpAgent: new http.Agent({ keepAlive: true }),
  httpsAgent: new https.Agent({ keepAlive: true }),

  // `proxy` 定義了代理伺服器的主機名、端口和協議。
  // 你可以使用常規的 `http_proxy` 和 `https_proxy` 環境變數。
  // 使用 `false` 可以禁用代理功能，同時環境變數也會被忽略。
  // `auth` 表示應使用 HTTP 基本認證連接到代理，並提供憑據。
  // 這將設置一個 `Proxy-Authorization` 請求頭，它會覆蓋 `headers` 中已存在的自訂 `Proxy-Authorization` 請求頭。
  // 如果代理伺服器使用 HTTPS，則必須設置 protocol 為 `https`
  proxy: {
    protocol: 'https',
    host: '127.0.0.1',
    port: 9000,
    auth: {
      username: 'mikeymike',
      password: 'rapunz3l'
    }
  },

  // 參見 https://axios-http.com/zh/docs/cancellation
  cancelToken: new CancelToken(function (cancel) {
  }),

  // `decompress` 指示是否應自動解壓回應主體。
  // 如果設置為 `true`，將會從所有解壓縮的回應物件中移除 'content-encoding' 頭
  // - 只有 Node (XHR 無法關閉解壓縮)
  decompress: true // 預設值
}
```