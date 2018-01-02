# Vue学习笔记2
## 1、Vue-directive自定义指令
Vue-directive:用于配合Vue中给的API定义自己想要的功能<br>
例：用Vue-directive自定一个改变颜色为红色的功能v-tianer(v-是命名的方式) <br>	
在body中：
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>practice</title>
</head>
<body>
	<div id="app">
		<div v-tianer="color">{{num}}</div>
		<p><button @click="jiafa">加法</button></p>
	</div>
</body>
</html>
```
我们在div中，绑定了我们自己设置的v-tianer <br>	
在script中：
```javascript
<script>
	Vue.directive('tianer',function(el,binding,vnode){
		el.style='color:'+binding.value;  //js中的style方法调用
	});
	var app = new Vue({
	el:'#app',
	data:{
		num:10,
		color:'red'
	},
	methods:{
		jiafa:function(){
			this.num++;
		}
	}
})
</script>
```
在script中，我们使用了Vue.directive方法来定义我们绑定的v-tianer，在function中的三个函数的意义分别为：  
- el:指令所绑定的元素，用于直接操作DOM（实际上该el在本例里指的是id为app的div）。  
- binding:一个对象，包含很多指令信息，里面有很多属性  
			binding.name指的是我们v-tianer中的tianer；  
			binding.value是data中的red；  
			binding.expression指的是绑定的属性；（因为v-tianer绑定data中的color，而通过binding.value可以获取到data中的color的值）。  
- v-node:Vue编译形成的一个虚拟节点。最后页面中的改变颜色的颜色为color中的red红色。  


自定义指令的五个生命周期（也可以叫钩子函数）<br>
分别是：bind，inserted，update，componentUpdate，unbind<br>
1. bind：只调用一次，指令第一次绑定到元素时使用，使用这个钩子函数可以定义一个绑定时只执行一次的初始化动作。<br>
2. inserted：被绑定元素插入父节点时调用（父节点存在时即可调用，不必存在于document中）。<br>
3. update：被绑定元素所在的模板更新（比如改变值的时候）时调用，无论绑定值是否变化，通过比较更新前后的绑定值，可以忽略不必要的模板变化<br>
4. componentUpdate：被绑定元素在模板完成一次更新周期（比如更新完成时）时调用。<br>
5. unbind：只调用一次，在于元素解绑时候调用。<br>
例：尝试在script中输出五个生命周期<br>
```javascript
Vue.directive('tianer',{
		bind:function(el,binding){//被绑定
		     console.log('1 - bind');
		     el.style="color:"+binding.value; //自定义指令的具体功能在开始绑定的时候写
		},
		inserted:function(){//绑定到节点
		      console.log('2 - inserted');
		},
		update:function(){//组件更新
		      console.log('3 - update');
		},
		componentUpdated:function(){//组件更新完成
		      console.log('4 - componentUpdated');
		},
		unbind:function(){//解绑
		      console.log('1 - bind');
		}
		});
```
关于解绑：<br>
解绑（unbind）的例子：<br>
在作用域的外边写一个按钮，用于解除绑定<br>
在body中：
```html
<body>
	<div id="app">
		<div v-tianer="color">改变颜色</div>
		<p>{{message}}</p>
	</div>
	<p><button onclick="unbind()">解绑</button></p>
</body>
```
在script中，使用原生的方式，创建unbind函数(放在vue实例化对象之外单独创建)：<br>
```
function unbinde(){
		app.$destroy();
	}
```
当点击解绑按钮的时候，则会解除原本加法按钮绑定的jiafa方法。并且，在此时控制台输出bind钩子函数的情况<br>


## 2、Vue-extend扩展实例构造器：
Vue-extend简单来说可以构造自己标签，并且为其进行自己的定义，经常用于配合Vue-component用于生成组件实例，并且挂载到自定义元素上。<br>
例：用Vue-extend来构建一个自己的tianer标签，tianer标签里可以展示自己特定的文字和拥有超链接功能：<br>
首先在script中构造：
```javascript
<script type="text/javascript">
	var tianerExtend = Vue.extend({  //使用Vue.extend来扩展实例
		template:"<p><a :href='tianerUrl'>{{textName}}</a></p>", //加入template属性，并且赋予其属性值，同时利用v-bind来绑定数据
		data:function(){
			return{
				textName:'天儿',
				tianerUrl:"https://github.com/Singleangel233"
			}
		}
	});
	new tianerExtend().$mount('#tianer');  //扩展完毕后，需要用Vue里面的$mount方法来直接绑定到指定元素中其中$mount()中的属性，可以是标签，可以是id，也可以是class名字
</script>
```
于是在body中，直接如下创建，可以得到应有的效果
```html
<body>
	<h1>Vue-extend扩展实例构造器</h1>
	<hr>
	<div id="tianer"></div>
</body>
```
## 3、Vue-set全局操作：
Vue.set 的作用就是在构造器外部**操作**构造器内部的数据、属性或者方法。<br>
在Vue中，如果对数组中对象中的数组进行下标的改动，Vue是无法监听到具体的下标的，本例使用点击按钮调用add()来对数字进行自加<br>
例：
```javascript
<script>
	function add(){
		// app.content++;    这是第一种可以实现++的方法
		// outdata.content++; 这是第二种，虽然只注释了这一句，但是后面一句起不了效果，如果没注释这一句，那么可以起到改变的效果
		app.arr[1]='ddd';   //直接越过outdata来用app获取到outdata中的arr属性，是不可以的
	}
	var outdata={
		content:1,
		// goods:20
		arr:['aaa','bbb','ccc']
	}
	var app = new Vue({
		el:"#app",
		data:outdata
	})
</script>
```
在这个时候，使用Vue-set可以起到对数组属性起到改动<br>
例：
```javascript
<script>
	function add(){
		// app.content++;
		// outdata.content++;
		// app.arr[1]='ddd';
		Vue.set(app.arr,1,'ddd'); //这样改动之后就可以改变app.arr[1]的值了
	}
	var outdata={
		content:1,
		// goods:20
		arr:['aaa','bbb','ccc']
	}
	var app = new Vue({
		el:"#app",
		data:outdata
	})
</script>
```
## 4、Vue的生命周期：
Vue一共有十个生命周期函数，我们可以在这十个生命周期函数中，操作数据和改变内容。
例：
```javascript
<script>
	var app = new Vue({
		el:"#app",
		data:{
			content:1
		},
		methods:{
			add:function(){
				this.content++;
			}
		}
	beforeCreate: function() {
		console.log('1-beforeCreate 初始化之前');
	},
	created: function() {
		console.log('2-created 创建完成');
	},
	beforeMount: function() {
		console.log('3-beforeMount 挂载之前');
	},
	mounted: function() {
		console.log('4-mounted 被挂载之后'); //载入页面时候1-4都会出现
	},
	beforeUpdate: function() {
		console.log('5-beforeUpdate 数据更新前'); //点击add按钮后出现在控制台
	},
	updated: function() {
		console.log('6-updated 被更新后'); //点击add按钮后出现在控制台
	},
	activated: function() {
		console.log('7-activated');
	},
	deactivated: function() {
		console.log('8-deactivated');
	},
	beforeDestroy: function() {
		console.log('9-beforeDestroy 销毁之前');
	},
	destroyed: function() {
		console.log('10-destroyed 销毁之后')
	}
	})
</script>
```
在body中设置好一个button用于销毁：<br>
例：
```html
<body>
	<div id="app"><p>{{content}}</p>
	<button @click="add">add</button>
	</div>
	<button onclick="app.$destroy()">destroy</button> <!-- 注意这个是放在了绑定的app之外执行，里面外面都可以 -->
</body>
```
当点击destory按钮后，则会在控制台输出第九个和第十个生命周期。

## 5、Vue的template模板
Vue的template有四种，其中有一种是在Vue-cli里面出现的，现在学习下面三种 <br>
第一种(在作用域对象里面写，适合小点的模板形式)：例：
```javascript
<script>
	var app = new Vue({
		el:"#app",
		data:{
			message:233
		},
		template:`  //注意要有``号来包裹
			<h2 style="color:red">选项模板</h2>
		`
	})
</script>
```
第二种(在body里面写template并且在作用域对象里面注明，适合直接制作好的):例：
```html
<body>
	<div id="app"><p>{{message}}</p></div>
	<template id="dd2">
		<h2 style="color: red">我是template标签模板</h2>
	</template>
</body>
</html>
<script src="./vue.js"></script>
<script>
	var app = new Vue({
		el:"#app",
		data:{
			message:233
		},
		template:"#dd2"		//注明了对应的标签id
	})
</script>
```
第三种(类似于js中的template模板，创建一个新的script标签来写,好处就是可以进行外部引用了)：例：
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Vue的template模板</title>
</head>
<body>
	<div id="app"><p>{{message}}</p></div>
<script type="x-template" id="dd3">  <!-- 这个x-template是专门要写表示模板的 -->
	<h2 style="color: red">我是script标签模板</h2> 
</script>
</body>
</html>
<script src="./vue.js"></script>
<script>
	var app = new Vue({
		el:"#app",
		data:{
			message:233
		},
		template:"#dd3"		//在这边引用
	})
</script>
```