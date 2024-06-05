# response schema

```javascript
{
  // `data` 由伺服器提供的回應
  data: {},

  // `status` 來自伺服器回應的 HTTP 狀態碼
  status: 200,

  // `statusText` 來自伺服器回應的 HTTP 狀態訊息
  statusText: 'OK',

  // `headers` 是伺服器的回應頭
  // 所有的 header 名稱都是小寫，而且可以使用方括號語法存取
  // 例如: `response.headers['content-type']`
  headers: {},

  // `config` 是 `axios` 請求的配置信息
  config: {},

  // `request` 是生成此回應的請求
  // 在 node.js 中它是最後一個 ClientRequest 實例 (in redirects)，
  // 在瀏覽器中則是 XMLHttpRequest 實例
  request: {}
}
```

