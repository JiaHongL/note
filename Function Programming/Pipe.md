# Pipe

## 描述

**pipe** 函數是一種常見的函數式編程技術，用於將多個函數串聯起來，按順序依次執行。每個函數的輸出作為下一個函數的輸入，最終得到一個結果。這種技術可以使代碼更具可讀性和模塊化。

假設有多個函數 f, g, h，數據 x 依次通過這些函數的處理

```javascript
pipe(f, g, h)(x);

// 等同於
h(g(f(x)));
```

## 實現

```javascript
const pipe = (...fns) => x => fns.reduce((v, f) => f(v), x);

```

## 使用

### pipe => add + multiply

```javascript
const pipe = (...fns) => x => fns.reduce((v, f) => f(v), x);

const add = x => x + 1;
const multiply = x => x * 2;

const pipedFunction = pipe(add, multiply);
console.log(pipedFunction(5)); // Output: 12

// 拆解
// const pipedFunction = pipe(add, multiply); // 回傳 function (x => fns.reduce((v, f) => f(v), x))， closure 保留 fns (add, multiply 函數)
// pipedFunction(5) // 等同於 fns.reduce((v, f) => f(v), 5)
```

## compose

**compose** 函數與 **pipe** 函數相似，不同之處在於 **compose** 函數是從右到左執行函數。

```javascript
compose(f, g, h)(x);

// 等同於
f(g(h(x)));
```

```javascript
const compose = (...fns) => x => fns.reduceRight((v, f) => f(v), x);
```

### compose => add + multiply

```javascript
const compose = (...fns) => x => fns.reduceRight((v, f) => f(v), x);

const add = x => x + 1;
const multiply = x => x * 2;

const composedFunction = compose(add, multiply);
console.log(composedFunction(5)); // Output: 11
```
