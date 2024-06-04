# 11 Navigation Failures

可能會失敗的原因

- 使用者已經位於他們正在嘗試導航到的頁面。
- 一個導航守衛通過調用 return false 中斷了這次導航。
- 當前的導航守衛還沒有完成時，一個新的導航守衛出現了。
- 一個導航守衛通過返回一個新的位置，重定向到其他地方（例如，return '/login'）。
- 一個導航守衛拋出了一個錯誤。

## 檢查導航是否成功

```js
const navigationResult = await router.push('/my-profile')

if (navigationResult) {
  // 导航被阻止
} else {
  // 导航成功 (包括重新导航的情况)
  this.isMenuOpen = false
}
```

## 檢查導航是否被取消

```js
import { NavigationFailureType, isNavigationFailure } from 'vue-router'

// 嘗試離開一個正在編輯的文章
const failure = await router.push('/articles/2')

// 可能導航被取消
if (isNavigationFailure(failure, NavigationFailureType.aborted)) {
  showToast('You have unsaved changes, discard and leave anyway?')
}
```

## global navigation guards fail

```js
router.afterEach((to, from, failure) => {
  if (failure) {
    sendToAnalytics(to, from, failure)
  }
})
```

> 在 `afterEach` 中檢查導航是否失敗，並且發送給分析服務。

## 判斷是哪一種導航失敗

正如我們在一開始所說的，有不同的情況會導致導航的中止，所有這些情況都會導致不同的導航故障（Navigation Failure）。它們可以用 isNavigationFailure 和 NavigationFailureType 來區分。總共有三種不同的類型：

- aborted（中止）： 在導航守衛中返回 false 中斷了本次導航。
- cancelled（取消）：在當前導航完成之前又有了新的導航。比如，在等待導航守衛的過程中又調用了 router.push。
- duplicated（重複）：導航被阻止，因為我們已經在目標位置了。

```js
import { createRouter, createWebHistory, isNavigationFailure, NavigationFailureType } from 'vue-router';

const router = createRouter({
  history: createWebHistory(),
  routes: [
    // 定義路由
  ],
});

router.push('/some-path').catch((failure) => {
  if (isNavigationFailure(failure, NavigationFailureType.aborted)) {
    console.log('導航被中止');
  } else if (isNavigationFailure(failure, NavigationFailureType.cancelled)) {
    console.log('導航被取消');
  } else if (isNavigationFailure(failure, NavigationFailureType.duplicated)) {
    console.log('導航重複');
  } else {
    console.log('其他導航失敗', failure);
  }
});
```

## Navigation Failures's properties

```js
router.push('/admin').then(failure => {
  if (isNavigationFailure(failure, NavigationFailureType.aborted)) {
    failure.to.path // '/admin'
    failure.from.path // '/'
  }
})
```

> 這個例子中，我們可以看到 `to` 和 `from` 屬性，它們分別代表了導航失敗的目標位置和起始位置。

## Detecting Redirections

```js
await router.push('/my-profile')
if (router.currentRoute.value.redirectedFrom) {
  // redirectedFrom is resolved route location like to and from in navigation guards
}
```

> 這個例子中，我們可以看到 `redirectedFrom` 屬性，它代表了重定向的起始位置。