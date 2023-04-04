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

