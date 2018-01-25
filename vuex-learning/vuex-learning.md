#Vue学习笔记7-vuex
## 7-1、vuex的安装和配置，以及创建实例
vuex的含义：用于状态管理。主要是数据的共享，比如data里的数据要给多个vue组件使用，这时候就可以使用vuex来管理共享的数据。<br>
vuex的使用：一般用于比较大型的项目，在需要用到的时候自然就会用到。<br>
<br>
**安装vuex** <br>
首先使用vue-cli初始化项目文件，然后打开命令行，在项目根目录下安装vuex。<br>
安装的命令：```npm install vuex --save```<br>
其中--save是对其注入依赖，用于生产环境的意思。<br>
安装完毕后可以在packge.json里查看到vuex的版本。<br>
<br>

**创建一个vuex相关实例**<br>
**第一步**<br>
首先在初始化项目根目录下的src文件下创建一个vuex文件夹，里面创建一个store.js文件用于管理data。<br>
创建后的store.js，自己添加初始内容为：<br>
```javascript
import Vue from 'vue';    //加载vue
import Vuex from 'vuex';  //加载vuex
Vue.use(Vuex); 			  //使用vuex
```
总结：注意大小写。<br>
这次的实例需求为，表示一个数字，然后通过按钮能够对数字进行加减的计算。<br>
这一次我们要使用store.js来实现这个需求。<br>
创建数据，创建方法，都是在store.js中实现。<br>
例，在store.js中：<br>
```javascript
import Vue from 'vue';
import Vuex from 'vuex';
Vue.use(Vuex);

const state={
	count:3
}				//使用es6创建常量对象，储存值

const mutations={
	add(state){
		state.count++;
	},
	reduce(state){
		state.count--;
	}
}				//使用mutations来创建对象，里面储存方法

export default new Vuex.Store({
	state,
	mutations
})				//创建好了后，需要使用这个来进行暴露，也可以理解为使用
```
注意：<br>
1.state这个名字和mutations这个名字最好就按照这个写。<br>
2.state里可以理解为存储的是值，mutation里面可以理解为存储的是方法。<br>
3.在这个实例中，mutation里面方法传入的值是state对象。<br>
4.创建完毕后，要使用```export default new Vuex.Store```来暴露使用。<br>
<br>
**第二步**<br>
然后创建一个组件模板，用于搭配使用，我们这次创建为Count.vue模板。<br>
在component中的Count.vue：
```
<template>
	<div>
		<h2>{{message}}</h2>
		<hr/>
		<h3>{{$store.state.count}}</h3>   //使用插值的方式传入了count值，注意写法
		<p>
			<button @click="$store.commit('add')">+</button >
			<button @click="$store.commit('reduce')">-</button>
		</p>   //创建了两个按钮，调用了mutation里的方法来实现自加自减的功能
	</div>
</template>

<script>
import store from '@/vuex/store';  //装载store，这一步特别关键，不能漏掉，不然识别不出
	export default {
		data(){
			return{
				message:'hello,vuex'
			}
		},
		store
	}
</script>
```
注意：<br>
1.如果要插入store.js中的值，必须要使用插值的形式。<br>
2.如果要引用store.js中的方法，必须要使用$store.commit('')的方式，commit用于实现，而且注意里面有单引号，不加单引号不行。<br>