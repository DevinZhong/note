---
title: Vue文档
copyright: true
date: 2016-09-28 15:43:46
tags:
  - Vue.js
  - JavaScript
---

## vuex
```js
// vuex/mutation-types.js
export const SOME_MUTATION = 'SOME_MUTATION'

// 简易Action工厂
function makeAction(type) {
  return ({ dispatch }, ...args) => {
    dispatch(type, ...args)
  }
}
```


Vue.js 组件 API 来自三部分——prop，事件和 slot：
- prop 允许外部环境传递数据给组件；
- 事件 允许组件触发外部环境的 action；
- slot 允许外部环境插入内容到组件的视图结构内。
```html
<my-component
  :foo="baz"
  :bar="qux"
  @event-a="doThis"
  @event-b="doThat">
  
  <img src>
  <p>Hello!</p>
</my-component>
```

## 基本规则

### 单次插值
```js
<span>This will never change: {{* msg }}</span>
```

### 插入html
内容以 HTML 字符串插入——数据绑定将被忽略。
```js
<div>{{{ raw_html }}}</div>
```

### 定义与使用过滤器
```js
Vue.filter('reverse', function (value) {
    return value.split('').reverse().join('')
})
{{message | reverse | uppercase}}
```

### 自带属性和方法
```js
vm.$data === data // -> true
vm.$el === document.getElementById('example') // -> true
// $watch 是一个实例方法
vm.$watch('a', function (newVal, oldVal) {
  // 这个回调将在 `vm.a`  改变后调用
})
```


## 计算属性
```js
// ...
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
// ...
```


## 事件

### 事件内联表达式访问原生DOM事件
```js
// ...
methods: {
  say: function (msg, event) {
    // 现在我们可以访问原生事件对象
    event.preventDefault()
  }
}
```


### 事件修饰符
```html

<a>

<form v-on:submit.prevent></form>

<div>...</div>

<div>...</div>
```

### 全部的按键别名：
- enter
- tab
- delete
- esc
- space
- up
- down
- left
- right


### 自定义按键别名
```js
Vue.directive('on').keyCodes.f1 = 112
```


## 参数特性
```html

<input v-model="msg" lazy>

<input v-model="age" number>


<input v-model="msg" debounce="500">
```

```css
.bounce-transition {
  display: inline-block; /* 否则 scale 动画不起作用 */
}
.bounce-enter {
  animation: bounce-in .5s;
}
.bounce-leave {
  animation: bounce-out .5s;
}
@keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.5);
  }
  100% {
    transform: scale(1);
  }
}
@keyframes bounce-out {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.5);
  }
  100% {
    transform: scale(0);
  }
}
```

## DOM模板的限制
- a 不能包含其它的交互元素（如按钮，链接）
- ul 和 ol 只能直接包含 li
- select 只能包含 option 和 optgroup
- table 只能直接包含 thead, tbody, tfoot, tr, caption, col, colgroup
- tr 只能直接包含 th 和 td


## prop

注意如果 prop 是一个对象或数组，是按引用传递。在子组件内修改它会影响父组件的状态，不管是使用哪种绑定类型。


### type构造器种类
- String
- Number
- Boolean
- Function
- Object
- Array
- instanceof 自定义构造器


### prop验证示例
```js
Vue.component('example', {
  props: {
    // 基础类型检测 （`null` 意思是任何类型都可以）
    propA: Number,
    // 多种类型 (1.0.21+)
    propM: [String, Number],
    // 必需且是字符串
    propB: {
      type: String,
      required: true
    },
    // 数字，有默认值
    propC: {
      type: Number,
      default: 100
    },
    // 对象/数组的默认值应当由一个函数返回
    propD: {
      type: Object,
      default: function () {
        return { msg: 'hello' }
      }
    },
    // 指定这个 prop 为双向绑定
    // 如果绑定类型不对将抛出一条警告
    propE: {
      twoWay: true
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        return value > 10
      }
    },
    // 转换函数（1.0.12 新增）
    // 在设置值之前转换值
    propG: {
      coerce: function (val) {
        return val + '' // 将值转换为字符串
      }
    },
    propH: {
      coerce: function (val) {
        return JSON.parse(val) // 将 JSON 字符串转换为对象
      }
    }
  }
})
```