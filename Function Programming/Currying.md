# Currying

## 描述

將一個接受 n 個參數的 function => 轉變成 n 個只接受一個參數的 function

```javascript
f(a,b,c) => g(a)(b)(c)
```

before currying

```javascript
functionadd(x, y, z) { 
    return x + y + z; 
} 
add(1, 2, 3); // 6 
```

curried function

```javascript
function curriedAdd(x) {
  return function(y) {
    return function(z) {
      return x + y + z;
    };
  };
}

curriedAdd(1)(2)(3); // 6

// arrow function
const curriedAdd = (x) => (y) => (z) => x + y + z;
or
const curriedAdd = x => y => z => x + y + z;
```

拆解 curriedAdd(1)(2)(3)

```javascript
const add = x => y => z => x + y + z;
// 等同於
// const add = x => {
//   return y => {
//     return z => {
//       return x + y + z;
//     };
//   };
// };

aad // 得到 function (x => y => z => x + y + z)
add(1) // 可以獲得 function (y => z => 1 + y + z)
add(1)(2) // 可以獲得function (z => 1 + 2 + z)
add(1)(2)(3) // 執行 function 1 + 2 + 3 = 6
```

> 使用 closure 來記住每次呼叫的參數，直到所有參數都被傳遞進去，才會執行原本的 function

## 其他範例
  
### 格式化日期

before currying

```javascript
function formatDate(format, date) {
  const options = { year: 'numeric', month: 'long', day: 'numeric' };
  return new Intl.DateTimeFormat(format, options).format(new Date(date));
}

console.log(formatDate('en-US', '2024-06-12')); // June 12, 2024
console.log(formatDate('en-GB', '2024-06-12')); // 12 June 2024
```

curried function

```javascript
const formatDate = format => date => {
  const options = { year: 'numeric', month: 'long', day: 'numeric' };
  return new Intl.DateTimeFormat(format, options).format(new Date(date));
};

const formatUSDate = formatDate('en-US');
console.log(formatUSDate('2024-06-12')); // June 12, 2024

const formatUKDate = formatDate('en-GB');
console.log(formatUKDate('2024-06-12')); // 12 June 2024
```

### 篩選數組

before currying

```javascript
function filter(predicate, arr) {
  return arr.filter(predicate);
}

const isEven = num => num % 2 === 0;
console.log(filter(isEven, [1, 2, 3, 4, 5])); // [2, 4]
```

curried function

```javascript
const filter = predicate => arr => arr.filter(predicate);

const isEven = num => num % 2 === 0;
const getEvenNumbers = filter(isEven);

console.log(getEvenNumbers([1, 2, 3, 4, 5])); // [2, 4]
```
