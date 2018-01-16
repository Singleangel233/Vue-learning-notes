# Vue学习笔记5-Vue-router
## 6-1、vue-router的安装和含义
vue-router，简称路由，是Vue全家桶中的一个组件，主要是用于制作单页应用。<br>
单页应用的意义在于不使用```<a href=""></a>```标签，我们就使用一个页面来管理应用。<br>

**安装：** <br>
如果在使用vue-cli初始化项目过，那么就会在命令行里提示是否安装vue-router。<br>
否则在命令行里面输入```npm install vue-router --save-dev``` 进行安装<br>


**关于src文件夹中的router文件中的Index.js解读** <br>
```javascript
import Vue from 'vue'  //引入了vue组件包
import Router from 'vue-router'   //引入了vue-router组件
import HelloWorld from '@/components/HelloWorld'   //引入的是components文件夹中的HelloWorld.vue文件，这个文件也可以叫做模板

Vue.use(Router)  //全局使用router组件

export default new Router({   //写了一个接口定义router
  routes: [   //配置routes，格式为数组，里面为对象
    {
      path: '/',     //路径
      name: 'HelloWorld',     
      component: HelloWorld   //组件，对应上面的components/HelloWorld
    }
  ]
})
```

**创建自己的一个hi.vue页面** <br>
首先应该在components文件下创建一个hi.vue。<br>
vue的文件格式首先得满足格式：
```
<template></template>
<script></script>
<style></style>
```
例：<br>
hi.vue <br>
```vue
<template>
	<div>
		<h2>{{message}}</h2>
	</div>
</template>
<script>
	export default{
		name:'hi',
		data(){
			return{
				message:"this is my hi page"
			}
		}
	}
</script>
<style>
	
</style>
```