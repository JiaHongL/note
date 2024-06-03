# 04 Class & Style

## 綁定 class

### class 綁定物件

```template
const isActive = ref(true)
const hasError = ref(false)

<div :class="{ active: isActive, 'text-danger': hasError }"></div>
```

### 綁定 computed

```template
const isActive = ref(true)
const error = ref(null)

const classObj = computed(() => {
  return {
    active: isActive.value && !error.value,
    'text-danger': error.value
  }
})

<div :class="classObj"></div>
```

### class 綁定陣列

```template
const activeClass = ref('active')
const errorClass = ref('text-danger')

<div :class="[activeClass, errorClass]"></div>
```

### 綁定兩者

```template
const activeClass = ref('active')
const errorClass = ref('text-danger')

<div :class="[activeClass, { 'text-danger': hasError }]"></div>
```

### 在元件上

```template
<!-- 子組件模板 -->
<p class="foo bar">Hi!</p>

<!-- 在使用組件時 -->
<MyComponent class="baz boo" />

<!-- 會被渲染成： -->
<p class="foo bar baz boo">Hi!</p>
```

> 元件並不會多一層

### 若元件內有多個根元素

```template
<MyComponent :class="{ active: isActive }" />

<!-- MyComponent 模板使用 $attrs 時 -->
<p :class="$attrs.class">Hi!</p>
<span>This is a child component</span>

<!-- 會被渲染成： -->
<p class="baz">Hi!</p>
<span>This is a child component</span>
```

## 綁定 style

### style 綁定物件

```template
const style = ref({
  color: 'red',
  fontSize: '14px'
})

<div :style="style"></div>


const fontSize = ref(10);

<div :style="{ 'font-size': fontSize + 'px' }"></div>
```

### style 綁定陣列

```template

const style = ref({
  color: 'red',
  fontSize: '14px'
})

const baseStyles = ref({
  color: 'blue'
})

<div :style="[baseStyles, style]"></div>
```