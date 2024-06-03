# Vue 筆記

## 屬性綁定

```html
<div v-bind:id="dynamicId"></div>

// 簡寫
<div :id="dynamicId"></div>

// 同名簡寫
<!-- 與 :id="id" 相同 -->
<div :id></div>

<!-- 這也同樣有效 -->
<div v-bind:id></div>

// boolean 屬性
<button :disabled="isButtonDisabled">Button</button>

// 動態綁定多個值
const objectOfAttrs = {
  id: 'container',
  class: 'wrapper'
}
<div v-bind="objectOfAttrs"></div>

JS Expressions
{{ number + 1 }}
{{ ok ? 'YES' : 'NO' }}
{{ message.split('').reverse().join('') }}
<div :id="`list-${id}`"></div>

// 調用函數
<time :title="toTitleDate(date)" :datetime="date">
  {{ formatDate(date) }}
</time>
```

```html
<a v-bind:href="url"> ... </a>
<!-- 簡寫 -->
<a :href="url"> ... </a>

<a v-on:click="doSomething"> ... </a>
<!-- 簡寫 -->
<a @click="doSomething"> ... </a>
```

## 動態參數

```html
<!--
注意，參數表達式有一些約束，
參見下面“動態參數值的限制”與“動態參數語法的限制”章節的解釋
-->
<a v-bind:[attributeName]="url"> ... </a>

<!-- 簡寫 -->
<a :[attributeName]="url"> ... </a>
```
