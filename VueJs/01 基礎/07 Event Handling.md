# 07 Event Handling

## Event Binding

```html
v-on:click="handler" 
```

或

```html
@click="handler"
```

> @ 是 v-on 的縮寫

## Method in Inline Handler

```html
<button @click="add(1)">Add 1</button>
```

```js
const add = (num) => {
  count += num
}
```

## $event

```html
<button @click="add(1, $event)">Add 1</button>
```

```js
const add = (num, event) => {
  count += num
  console.log(event)
}
```

或使用 arrow function

```html
<button @click="(event) => add('Form cannot be submitted yet.', event)">Add 1</button>
```

```js
const add = (num, event) => {
  count += num
  console.log(event)
}
```

## Event Modifiers 事件修飾符

- `.stop`: 阻止事件冒泡，即不再向父元素傳遞。
- - `.self`: 只當事件是從元素本身觸發時才觸發
- `.capture`: 添加事件監聽器時使用事件捕獲模式，原本由內部元素觸發的事件，將先在祖先元素上觸發。 inner ->  outer 變成 outer -> inner。
- `.prevent`: 阻止事件的默認行為
- `.once`: 事件只觸發一次
- `.passive`: 用來優化滾動事件的性能。當你在 Vue.js 或 JavaScript 中使用滾動事件時，默認情況下事件處理函數是會阻塞頁面的渲染的。這可能會導致滾動卡頓。passive 修飾符告訴瀏覽器事件處理函數不會調用 event.preventDefault()，因此瀏覽器可以在滾動繼續的同時執行事件處理函數，從而提高滾動性能。

```html
<form @submit.prevent="onSubmit">...</form>
```

## Key Modifiers 按鍵修飾符

- `.enter`
- `.tab`
- `.delete` (捕獲 "刪除" 和 "退格" 鍵)
- `.esc`
- `.space`
- `.up`
- `.down`
- `.left`
- `.right`

```html
<input @keyup.enter="submit">
```

## System Modifier Keys 系統修飾鍵

- `.ctrl`：在 Mac 上是 Control 鍵，在 Windows 上是 Ctrl 鍵。
- `.alt`：在 Mac 上是 Option 鍵，在 Windows 上是 Alt 鍵。
- `.shift`
- `.meta`: 在 Mac 上是 Command 鍵，在 Windows 上是 Windows 鍵。

```html
<!-- Ctrl + Click -->
<button @click.ctrl="onClick">A</button>
```

```html
<!-- Alt + Enter -->
<input @keyup.alt.enter="onAltEnter">
```

```html
<!-- Ctrl + 點擊 -->
<button @click.ctrl="onClick">A</button>
```

## .exact 修飾符

```html
<!-- 當按下 Ctrl 時，即使同時按下 Alt 或 Shift 也會觸發 -->
<button @click.ctrl="onClick">A</button>

<!-- 僅當按下 Ctrl 且未按任何其他鍵時才會觸發 -->
<button @click.ctrl.exact="onCtrlClick">A</button>

<!-- 僅當沒有按下任何系統按鍵時觸發 -->
<button @click.exact="onClick">A</button>
```

## Mouse Button Modifiers 滑鼠按鈕修飾符

- `.left`
- `.right`
- `.middle`

```html
<!-- 只有在滑鼠左鍵點擊時才觸發 -->
<button @click.left="onClick">A</button>

<!-- 只有在滑鼠右鍵點擊時才觸發 -->
<button @click.right="onClick">B</button>

<!-- 只有在滑鼠中鍵點擊時才觸發 -->
<button @click.middle="onClick">C</button>
```
