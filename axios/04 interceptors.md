# 04 interceptors

## interceptors.request & interceptors.response

```js
// 添加請求攔截器
axios.interceptors.request.use(function (config) {
    // 在發送請求之前做些什麼
    return config;
  }, function (error) {
    // 對請求錯誤做些什麼
    return Promise.reject(error);
  });

// 添加回應攔截器
axios.interceptors.response.use(function (response) {
    // 狀態碼在 2xx 範圍內的會觸發這個函式
    // 對回應資料做些什麼
    return response;
  }, function (error) {
    // 狀態碼超出 2xx 範圍的會觸發這個函式
    // 對回應錯誤做些什麼
    return Promise.reject(error);
  });
```

## use & remove

```js
const myInterceptor = axios.interceptors.request.use(function () {/*...*/});
axios.interceptors.request.eject(myInterceptor);
```

> 你可以使用 `axios.interceptors.request.use` 添加攔截器，並使用 `axios.interceptors.request.eject` 移除它。

## 常見的 axios 攔截器用法 (authorization & error handling & refresh token)

```js
import axios from 'axios';

let isRefreshing = false;
let failedQueue = [];

const processQueue = (error, token = null) => {
  failedQueue.forEach(prom => {
    if (error) {
      prom.reject(error);
    } else {
      prom.resolve(token);
    }
  });

  failedQueue = [];
};

const instance = axios.create();

instance.interceptors.request.use(
  config => {
    const token = localStorage.getItem('token');
    if (token) {
      config.headers['Authorization'] = `Bearer ${token}`;
    }
    return config;
  },
  error => {
    return Promise.reject(error);
  }
);

instance.interceptors.response.use(
  response => {
    return response;
  },
  async error => {
    const originalRequest = error.config;

    if (error.response.status === 401 && !originalRequest._retry) {
      if (originalRequest.url.includes('/login')) {
        return Promise.reject(error);
      }

      if (isRefreshing) {
        return new Promise(function(resolve, reject) {
          failedQueue.push({ resolve, reject });
        })
        .then(token => {
          originalRequest.headers['Authorization'] = 'Bearer ' + token;
          return axios(originalRequest);
        })
        .catch(err => {
          return Promise.reject(err);
        });
      }

      originalRequest._retry = true;
      isRefreshing = true;

      return new Promise(function(resolve, reject) {
        axios.post('/auth/refresh', { token: localStorage.getItem('refreshToken') })
          .then(({ data }) => {
            localStorage.setItem('token', data.token);
            localStorage.setItem('refreshToken', data.refreshToken);
            instance.defaults.headers.common['Authorization'] = 'Bearer ' + data.token;
            originalRequest.headers['Authorization'] = 'Bearer ' + data.token;
            processQueue(null, data.token);
            resolve(axios(originalRequest));
          })
          .catch((err) => {
            processQueue(err, null);
            localStorage.removeItem('token');
            localStorage.removeItem('refreshToken');
            window.location.href = '/login'; // 重定向到登出頁面或登入頁面
            reject(err);
          })
          .finally(() => {
            isRefreshing = false;
          });
      });
    }

    if (error.response.status >= 400 && error.response.status < 500) {
      if (!originalRequest.url.includes('/login')) {
        // 處理 4xx 錯誤，排除 login api
        return Promise.reject(error);
      }
    }

    return Promise.reject(error);
  }
);

export default instance;
```

這段代碼完成了以下功能：

- 在請求的 headers 中添加 token。
- 當遇到 401 錯誤時，會進行 token 的刷新。
- 如果刷新失敗，會登出用戶；如果成功，會儲存新的 tokens。
- 排除 login api 的 4xx 錯誤處理。
- 在進行刷新時，其他 API 請求會先跳過，待刷新完成後重新發送這些請求。