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
<br>
**第三步**<br>
然后需要在router文件中的index.js声明做好的Count.vue组件。<br>
在index.js中：<br>
```javascript
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'
import Count from '@/components/Count'

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/',
      name: 'HelloWorld',
      component: HelloWorld
    },
    {
    	path:'/count',
    	component:Count
    }
  ]
})
```
这边载入了count已经创建了一个新的routes元素，里面声明了Count.vue组件。<br>
<br>
**最后运行**<br>
最后在浏览器中输入到count页面，就可以看得到页面具体的内容。<br>


## 7-2、state访问状态对象
之前所了解了store.js（数据仓库）里的各种属性，现在我们来了解关于里面的state属性。<br>
首先state是一个状态对象，也可以说它是用于存储数据的对象。<br>
这次我们要在Count.vue中通过三种方式，使得插值的格式变得简单也可以展现store.js中的count的值。<br>
**第一种：**<br>
例，在Count.vue中：<br>
```vue
<template>
	<div>
		<h2>{{message}}</h2>
		<hr/>
		<h3>{{$store.state.count}}--{{count}}</h3>
		<p>
			<button @click="$store.commit('add')">+</button >
			<button @click="$store.commit('reduce')">-</button>
		</p>
	</div>
</template>

<script>
import store from '@/vuex/store';
	export default {
		data(){
			return{
				message:'hello,vuex'
			}
		},
		computed:{
			count(){
				return this.$store.state.count;
			}  					//使用了computed这个方法将值赋到了count这个上面
		},
		store
	}
</script>
```
这里使用了computed这个方法，将this.$store.state.count的值赋值到了count上。<br>
插值只需要直接插入{{count}}也能达到效果。<br>

**第二种：**<br>
使用mapstate来达到效果。<br>
首先需要在Count.vue的script标签内载入mapstate。<br>
然后在computed里面使用mapState代替。<br>
例，在Count.vue中：<br>
```vue
<template>
	<div>
		<h2>{{message}}</h2>
		<hr/>
		<h3>{{$store.state.count}}--{{count}}</h3>
		<p>
			<button @click="$store.commit('add')">+</button >
			<button @click="$store.commit('reduce')">-</button>
		</p>
	</div>
</template>

<script>
import store from '@/vuex/store';
import { mapState } from 'vuex';


	export default {
		data(){
			return{
				message:'hello,vuex'
			}
		},
		computed:mapState({
			count:state=>state.count    //这里使用了ES6的箭头函数
		}),
		store
	}
</script>
```
注意：<br>
1.需要在script中加载mapState，要注意加载的时候一定要有花括号，路径为vuex。<br>
2.这里使用了ES6箭头函数```count:sate=>state.count```这个用ES5来解释如同为: <br>
```javascript
count:function(state){
	return state.count;
}
```
然后也可以显示插值里的count的值。<br>
<br>
**第三种：(敲简单)** <br>
在mapState中不用对象的形式，使用数组的形式，直接里面值为'count'也可以达到效果。这一种在平日中使用比较多<br>
例，在Count.vue中：<br>
```vue
<template>
	<div>
		<h2>{{message}}</h2>
		<hr/>
		<h3>{{$store.state.count}}--{{count}}</h3>
		<p>
			<button @click="$store.commit('add')">+</button >
			<button @click="$store.commit('reduce')">-</button>
		</p>
	</div>
</template>

<script>
import store from '@/vuex/store';
import { mapState } from 'vuex';


	export default {
		data(){
			return{
				message:'hello,vuex'
			}
		},
		computed:mapState(['count']),  //这里的写法发生了变化
		store
	}
</script>
```
注意：mapState()中加入的是一个数组，而且数组中的值为有单引号为'count'，这样才能识别到store.js中的count。<br>


## 7-3、mutations改变状态
之前我们绑定了在store.js中的mutation中的方法，这次对mutation进入深入的学习。<br>
**1.在方法中如何加入值：**<br>
这个操作需要在store.js中加入值。<br>
例，在store.js中：
```javascript
import Vue from 'vue';
import Vuex from 'vuex';
Vue.use(Vuex);

const state={
	count:3
}

const mutations={
	add(state,n){
		state.count+=n;   //注意这里传入了一个为n的值，直接加在state后面
	},
	reduce(state){
		state.count--;
	}
}

export default new Vuex.Store({
	state,
	mutations
})
```
然后在Count.vue中，就可以直接对其进行引用了。<br>
例，在Count.vue中：
```vue
<template>
	<div>
		<h2>{{message}}</h2>
		<hr/>
		<h3>{{count}}</h3>
		<p>
			<button @click="$store.commit('add',10)">+</button>  //这里传入数值，在commit()中加入数值
			<button @click="$store.commit('reduce')">-</button>
		</p>
	</div>
</template>

<script>
import store from '@/vuex/store';
import { mapState } from 'vuex';


	export default {
		data(){
			return{
				message:'hello,vuex'
			}
		},
		computed:mapState({
			count:state=>state.count
		}),
		store
	}
</script>
```
注意：要在对应的commit()中传入数值。<br>
<br>
**2.简写方法：**<br>
通过在Count.vue中修改，可以简写绑定在标签里的方法。<br>
例，在Count.vue中：
```vue
<template>
	<div>
		<h2>{{message}}</h2>
		<hr/>
		<h3>{{count}}</h3>
		<p>
			<button @click="$store.commit('add',10)">+</button >
			<button @click="reduce">-</button>  //可以简写了，注意下面配置
		</p>
	</div>
</template>

<script>
import store from '@/vuex/store';
import { mapState,mapMutations } from 'vuex'; //加载mapMutations，用逗号隔开


	export default {
		data(){
			return{
				message:'hello,vuex'
			}
		},
		computed:mapState({
			count:state=>state.count
		}),
		methods:mapMutations(['add','reduce']),  //要简写的方法加入到methods中
		store
	}
</script>
```
注意：<br>
1.首先要加载mapMutations，而且和mapState用逗号隔开<br>
2.加载完毕后，使用methods属性来进行要简写方法的配置，格式为数组，每个不同方法用单引号包裹，用逗号隔开。<br>
这样就可以简写绑定的方法了，而不用$store.commit()这类格式。<br>


## 7-4、getters过滤操作
使用getters可以对属性进行过滤操作，也就是每次执行方法前都会进行一遍过滤操作。<br>
可以理解为在store.js中的类似于computed。<br>
例如，我们创建一个过滤器，来实现无论加减都进行加10的功能。<br>
首先需要再store.js中配置getters。<br>
例，在store.js中：<br>
```javascript
import Vue from 'vue';
import Vuex from 'vuex';
Vue.use(Vuex);

const state={
	count:3
}

const mutations={
	add(state,n){
		state.count+=n;
	},
	reduce(state){
		state.count--;
	}
}

const getters={
	count:function(state){
		return state.count+=10;  //注意这里的写法
	}
}

export default new Vuex.Store({
	state,
	mutations,
	getters						//配置好后记得加入getters
})
```
注意：getters创建后，里面是对象的格式，然后再编写一个属性，属性值为一个匿名函数，可以使用ES6的箭头函数实现。<br>
例，使用箭头函数实现getters里面的匿名函数。<br>
```javascript
count.state=>state.count+=10;
```
然后在Count.vue中进行配置。<br>
例，在Count.vue中：<br>
```vue
<template>
	<div>
		<h2>{{message}}</h2>
		<hr/>
		<h3>{{count}}</h3>
		<p>
			<button @click="$store.commit('add',10)">+</button >
			<button @click="reduce">-</button>
		</p>
	</div>
</template>

<script>
import store from '@/vuex/store';
import { mapState,mapMutations } from 'vuex';


	export default {
		data(){
			return{
				message:'hello,vuex'
			}
		},
		computed:{                  //getters要配置在computed中
			...mapState(['count']),   
			count(){
				return this.$store.getters.count; 
			}	 //这是配置好了的getters
		},
		methods:mapMutations(['add','reduce']),
		store
	}
</script>
```
注意：<br>
1.getters要配置在computed属性中。<br>
2.因为之前使用了mapState来简写了count属性，如果再加computed则会被覆盖。
3.这里为了不覆盖，都写在computed中，所以这里要用到ES6的扩展运算符```...```。<br>
4.要使用扩展运算符，computed中的属性值要以对象来设置。<br>
例：<br>
```vue
computed:{
		...mapState(['count']),   
		count(){
				return this.$store.getters.count; 
}
```
原来不用扩展运算符之前是：<br>
```vue
computed:mapState(['count']),
```
这样完成后，在页面中，点击+或者-都可以实现加10，无论加或者减，都起到了过滤的效果。<br>
<br>
**简写getters：**<br>
与其它vuex属性相似，getters也有可以简写的办法。<br>
我们需要在Count.vue中配置。<br>
例，在Count.vue中：
```vue
<template>
	<div>
		<h2>{{message}}</h2>
		<hr/>
		<h3>{{count}}</h3>
		<p>
			<button @click="$store.commit('add',10)">+</button >
			<button @click="reduce">-</button>
		</p>
	</div>
</template>

<script>
import store from '@/vuex/store';
import { mapState,mapMutations,mapGetters } from 'vuex';  //添加载入mapGetters


	export default {
		data(){
			return{
				message:'hello,vuex'
			}
		},
		computed:{
			...mapState(['count']),
			...mapGetters(['count'])  //这里跟上面一样，使用扩展运算符，然后里面数组加字符串格式，count是在getters里定义好的
		},
		methods:mapMutations(['add','reduce']),
		store
	}
</script>
```
注意：<br>
1.要在import中载入mapGetters。<br>
2.使用扩展运算符来写，例：```...mapGetters(['count'])``` <br>
3.注意数组内是字符串，并且count是在getters定义好的count属性。<br>


## 7-5、actions异步修改状态