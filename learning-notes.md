# vue.js学习

## 学习参考

vue3中文文档：https://cn.vuejs.org/guide/introduction.html

## 学习内容

### 核心功能：声明式渲染

1、通过扩展于标准 HTML 的模板语法，我们可以根据 JavaScript 的状态来描述 HTML 应该是什么样子的。当状态改变时，HTML 会自动更新。

2、响应式布局

代码举例：

```javascript
<script> export default {  
data() {   
return {     
    message: 'Hello World!',     
	counter: {   
    count: 1     
    }   
		} 
	}
}
</script>

<template> 
<h1>{{ message }}</h1> 
<p>Count is: {{ counter.count }}</p>
</template>
```

### attribute绑定

mustache 语法 (即双大括号) 只能用于文本插值。为了给 attribute 绑定一个动态值，需要使用 `v-bind` 指令.

冒号后面的部分 (`:id`) 是指令的“参数”。此处，元素的 `id` attribute 将与组件状态里的 `dynamicId` 属性保持同步

```javascript
<div v-bind:id="dynamicId"></div>
//省略版
<div :id="dynamicId"></div>
```

### 事件监听

使用 `v-on` 指令监听 DOM 事件：

```javascript
<button v-on:click="increment">{{ count }}</button>
//简化版
<button @click="increment">{{ count }}</button>
```

在方法中，可以使用this来访问组件实例。组件实例会暴露data中声明的数据属性。可以通过改变这些属性的值来更新组件状态。

### 表单绑定

可以同时使用v-bind和v-on来在表单的输入元素上创建双向绑定：

```javascript
<script>
export default {
  data() {
    return {
      text: ''
    }
  },
  methods: {
    onInput(e) {
      this.text = e.target.value
    }
  }
}
</script>

<template>
  <input :value="text" @input="onInput" placeholder="Type here">
  <p>{{ text }}</p>
</template>
```

为了简化双向绑定，Vue 提供了一个 `v-model` 指令，它实际上是上述操作的语法糖。**下面这种写法比较舒服**

```javascript
<script>
export default {
  data() {
    return {
      text: ''
    }
  }
}
</script>

<template>
  <input v-model="text" placeholder="Type here">
  <p>{{ text }}</p>
</template>
```

### 条件渲染

使用 `v-if` 指令来有条件地渲染元素，也可以使用 `v-else` 和 `v-else-if` 来表示其他的条件分支：

```javascript
<h1 v-if="awesome">Vue is awesome!</h1>
<h1 v-else>Oh no 😢</h1>
```

### 列表渲染

可以使用 `v-for` 指令来渲染一个基于源数组的列表：

```javascript
<ul>
  <li v-for="todo in todos" :key="todo.id">
    {{ todo.text }}
  </li>
</ul>
```

这里的 `todo` 是一个局部变量，表示当前正在迭代的数组元素。它只能在 `v-for` 所绑定的元素上或是其内部访问，就像函数的作用域一样。

注意，我们还给每个 todo 对象设置了唯一的 `id`，并且将它作为特殊的key attribute绑定到每个 `<li>`。`key` 使得 Vue 能够精确的移动每个 `<li>`，以匹配对应的对象在数组中的位置。

更新列表有两种方式：

1. 在源数组上调用变更方法：

   ```javascript
   this.todos.push(newTodo) //this.todos.push({id:id++,text:this.newTodo})
   ```

2. 使用新的数组替代原数组：

   ```javascript
   this.todos = this.todos.filter(/* ... */) //this.todos=this.todos.filter(((t)=>t!==todo))
   ```

这里有一个简单的 todo 列表——试着实现一下 `addTodo()` 和 `removeTodo()` 这两个方法的逻辑，使列表能够正常工作！

## 计算属性

可以使用 `computed` 选项声明一个响应式的属性，它的值由其他属性计算而来：

```javascript
export default {
  // ...
  computed: {
    filteredTodos() {
      // 根据 `this.hideCompleted` 返回过滤后的 todo 项目
    }
  }
}

```

计算属性会自动跟踪其计算中所使用的到的其他响应式状态，并将它们收集为自己的依赖。计算结果会被缓存，并只有在其依赖发生改变时才会被自动更新。

**computed和methods区别：**

1. 调用方式不同。computed直接以对象属性方式调用，不需要加括号，而methods必须要函数执行才可以得到结果。
2. 绑定方式不同。methods与compute纯get方式都是单向绑定，不可以更改输入框中的值。compute的get与set方式是真正的双向绑定。
3. 是否存在缓存。methods没有缓存，调用相同的值计算还是会重新计算。competed有缓存，在值不变的情况下不会再次计算，而是直接使用缓存中的值
