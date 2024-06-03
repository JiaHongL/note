# 08 From Input Bindings

## 1. v-model

```js
const message = ref('Hello!')
```

```html
<input v-model="message" placeholder="Enter something">
<p>Message is: {{ message }}</p>
```

> v-model 會自動忽略任何表單元素上的 value、checked、selected 特性，並將 ref 的值作為元素的 value。

## 各式表單元素


### 1.1 Text

```js
const message = ref('Hello!')
```

```html
<input type="text" v-model="message" placeholder="Enter something">
```

### 1.2 Textarea

```js
const message = ref('Hello!')
```

```html
<textarea v-model="message" placeholder="Enter something"></textarea>
```

### 1.3 Checkbox (多個)

```js
const checked = ref(['A', 'B'])
```

```html
<input type="checkbox" value="A" v-model="checked">
<input type="checkbox" value="B" v-model="checked">
<input type="checkbox" value="C" v-model="checked">
```

### 1.4 Radio

```js
const picked = ref('A')
```

```html
<input type="radio" value="A" v-model="picked">
<input type="radio" value="B" v-model="picked">
```

### 1.5 Select

```js
const selected = ref('A')
```

```html
<select v-model="selected">
  <option value="A">A</option>
  <option value="B">B</option>
</select>
```

### 1.6 Multiple Select

```js
const selected = ref(['A', 'B'])
```

```html
<select multiple v-model="selected">
  <option value="A">A</option>
  <option value="B">B</option>
  <option value="C">C</option>
</select>
```

### 1.7 Custom Select

```js
const selected = ref('A')
```

```html
<select v-model="selected">
  <option value="A">A</option>
  <option value="B">B</option>
  <option value="C">C</option>
</select>
```

### 1.8 Custom Select with Object

```js
const selected = ref({ id: 1, name: 'A' })
```

```html
<select v-model="selected">
  <option :value="{ id: 1, name: 'A' }">A</option>
  <option :value="{ id: 2, name: 'B' }">B</option>
  <option :value="{ id: 3, name: 'C' }">C</option>
</select>
```

### 搭配 v-for 使用

```js
const options = ref([
  { id: 1, name: 'A' },
  { id: 2, name: 'B' },
  { id: 3, name: 'C' }
])
const selected = ref([options.value[0].id])
```

```html
<select v-model="selected">
  <option 
    v-for="option in options" 
    :value="option.key"
  >{{ option.name }}</option>
</select>
```

### true-value 和 false-value

```js
const checked = ref('yes')
```

```html
<input 
  type="checkbox" 
  v-model="checked" 
  true-value="yes" 
  false-value="no"
>
```

## 修飾符

### lazy 修飾符

```js
const message = ref('Hello!')
```

```html
<input v-model.lazy="message" placeholder="Enter something">
```

> lazy 修飾符會在 change 事件觸發時更新資料，而不是 input 事件。

### number 修飾符

```js
const age = ref(18)
```

```html
<input type="number" v-model.number="age">
```

> 會在 type="number" 的 input 元素上使用。
> 如果 parseFloat() 無法解析輸入值，則會返回原始值。
> number 修飾符會自動將輸入值轉換為數字。

### trim 修飾符

```js
const message = ref('Hello!')
```

```html
<input v-model.trim="message" placeholder="Enter something">
```

> trim 修飾符會自動過濾輸入值的前後空白。