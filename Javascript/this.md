# this 觀念

this 在 JavaScript 中是一個動態的上下文參考，它的值取決於函數被呼叫的方式。

```javascript
const obj = {
  name: "Alice",
  greet: function() {
    console.log('name:', this.name);
  },
  greet2: () => {
    console.log('name:', this.name);
  }
};

obj.greet(); // "Alice"
obj.greet2(); // undefined

const greet = obj.greet;
const greet2 = obj.greet2;

greet(); // undefined
greet2(); // undefined

// 使用 bind
const boundGreet = obj.greet.bind(obj);
const boundGreet2 = obj.greet2.bind(obj);

boundGreet(); // "Alice"
boundGreet2(); // undefined
```

在上面的範例中

- obj.greet() 中的 this 參考的是 obj 本身。
- obj.greet2() 中的 this 參考的是全域物件（在瀏覽器中為 window）。
- greet() 獨立呼叫時，this 會參考全域物件（在瀏覽器中為 window），在嚴格模式下則為 undefined。
- boundGreet，使用 bind 可以顯式綁定 this。
- 箭頭函數則捕捉定義時的 this，而非呼叫時的 this。

> obj.xxx() -> obj 是呼叫者，所以 this 會指向 obj
> xxx() -> xxx 是呼叫者，所以 this 會指向全域物件
> arrow function 捕捉定義時的 this，這邊的話就是全域物件，所以就算使用 bind 也無法改變 this 的指向。