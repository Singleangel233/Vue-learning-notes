# Vue学习笔记3(构造器的选项)
所谓选项，就是指的是在Vue作用域对象里的属性，这些格式像属性的东西我们称之为选项，选项用于处理各种事务情况<br>
## 1、propsData 选项
propsData选项常用于扩展(extend)中<br>
例：给自定义好的标签传入一个插值<br>
在script中：
```javascript
var headerExtend = Vue.extend({ //使用extend来扩展一个自定义的标签
		template:`<p >{{message}}-{{a}}</p>`,  //在模板当中传入了一个插值{{a}}
		data:function(){
			return{
				message:"hello,it is successful",
			}
		},
		props:['a']
	})
new headerExtend({propsData:{a:5}}).$mount("header");  //在headerEextend里添加参数，对象propsData，并且定义插值a，然后绑定给标签header
```
这样定义好后，在页面中显示为：hello,it is successful-5 <br>


## 2、computed 选项
computed选项用于对原数据进行改造输出<br>
例如对数组中的数据进行改造输出：<br>
在script中：
```javascript
<script>
	var newList = [
			{title:"ABC",date:"2018/1/2"},
			{title:"CDE",date:"2018/1/3"},
			{title:"FGH",date:"2018/1/4"}   
	]  //创建了一个数组对象，包含了三个对象元素
	var app = new Vue({
		el:"#app",
		data:{
			price:100,
			newList:newList //把外部的数组放入data中
		},
		computed:{
			newPrice:function(){
				return this.price=this.price+"￥"; //使用computed选项然后将方法写入进去，对data中的数据进行改造，返回到newPrice中
			},
			reverseNews:function(){
				return this.newList.reverse(); //array.reverse()方法是JS中可以反向输出数组中数据的位置，返回到reverseNesw数组中
			}
		}
	})
</script>
```
在body中，绑定好数据，并且使用v-for对数组里的数据进行循环并生成新格式放到页面中：
```html
<body>
	<h1>computed</h1>
	<hr>
	<div id="app">
		<p>{{newPrice}}</p>
		<p>
			<ul>
				<li v-for="item in reverseNews">{{item.title}}--{{item.date}}</li>
			</ul>
		</p>
	</div>
</body>
```
则页面生成对应的内容，整个数组的数据输出反转，newPrice的值对应发生的变化<br>

## 3、methods选项
在使用构造器的过程中，对标签元素使用v-on绑定事件，然后在构造器中使用methods选项来写入事件具体函数<br>
总结：methods有四个小知识点<br>
3-1：传值的写法<br>
在script中：
```javascript
<script>
	var app = new Vue({
		el:"#app",
		data:{
			message:1
		},
		methods:{
			add:function(num){ //这里传了一个num
				if(num!=''){   //判断num是否为空
					this.message+=num;
				}else{
					this.message++;
				}
			}
		}
	})
</script>
```
在body中绑定的方法就可以传入值的大小了<br>
例：在body中：<br>
```html
<body>
	<h1>methods option</h1>
	<hr>
	<div id="app">
		<p>{{message}}</p>
		<button @click="add(2)">add</button>
	</div>
</body>
```
则每次点击按钮就会加2br	
3-2：$event <br>
在Vue.js中，里面有一个设计好的$event，$event包含了很多很多的属性，跟原生js的 mouseEvent 相同，用于交互性特别强的项目。<br>
3-3：编写好的组件想使用methods中的方法<br>
例，在script中：
```javascript
script>
	var btn ={
		template:`<button>新增组件add</button>` //编写好一个外部组件
	}
	var app = new Vue({
		el:"#app",
		data:{
			message:1
		},
		conponents:{
			"btn"= btn  	//对编写好的组件进行引用
		},
		methods:{
			add:function(num){
				if(num!=''){
					this.message+=num;
				}else{
					this.message++;
				}
			}
		}
	})
```
如果btn组件想使用add方法，那么在body中的写法应为：<br>
```html
<body>
	<h1>methods option</h1>
	<hr>
	<div id="app">
		<p>{{message}}</p>
		<p><button @click="add(2)">add</button></p>
		<p><btn @click.native="add(2)"></btn></p>   //这边调用了native方法，是表示使用methods中原生的方法，如果写成与上个button一样绑定形式是不能实现add(2)的效果的
	</div>
</body>
```
3-4：外部调用methods里的方法<br>
例，在Vue作用域外部创建一个按钮标签然后实现add()方法<br>
在body中：
```html
<body>
	<h1>methods option</h1>
	<hr>
	<div id="app">
		<p>{{message}}</p>
		<button @click="add(2,$event)">add</button>
	</div>
	<button onclick="app.add(2)">外部的add按钮</button>  //注意这个在外部的按钮想要使用点击后的add()方法的写法
</body>
```

## 4、watch选项（两种写法）
watch选项是可以监控数据变化，并且可以设定好函数来处理监控的数据<br>
例，通过两个按钮分别升高和降低温度，并且监控具体的数值来给出对应的字段：<br>
在script中：
```javascript
<script>
	var clotharray = ["T恤短袖","针织衫和卫衣","羽绒服和秋裤"] //创建了一个数组
	var app = new Vue({
		el:"#app",
		data:{
			temp:20,
			cloth:"针织衫和卫衣"
		},
		methods:{  //绑定增加和减少的方法
			add:function(){
				this.temp+=5;
			},
			reduce:function(){
				this.temp-=5;
			}
		},
		 watch:{
		 	temp:function(newVal,oldVal){  //使用watch选项对temp进行监控，并且构造一个函数，参数是固定的，newVal是代表变化后的新值，oldVal代表旧值
				if(newVal>=30){
		 			this.cloth = clotharray[0];
		 		}else if(newVal<30 && newVal>=10){  //判断newVal的新值大小，根据值的不同让当前的cloth为clotharray数组中的元素
		 			this.cloth = clotharray[1];
		 		}else{
		 			this.cloth = clotharray[2];
				}
		 	}
		 }
	})

</script>
```
在body中：
```html
	<div id="app">
		<p>今日温度：{{temp}}°</p>
		<p>穿衣建议：{{cloth}}</p>
		<button @click="add">升高温度</button><button @click="reduce">降低温度</button>
	</div>
```
watch也可以写在构造器外边，最好写在构造器的下方，在外部使用watch监控也能达到一样的效果<br>
例，在script中：
```javascript
<script>
	var clotharray = ["T恤短袖","针织衫和卫衣","羽绒服和秋裤"]
	var app = new Vue({
		el:"#app",
		data:{
			temp:20,
			cloth:"针织衫和卫衣"
		},
		methods:{
			add:function(){
				this.temp+=5;
			},
			reduce:function(){
				this.temp-=5;
			}
		}	
	})
	app.$watch('temp',function(newVal,oldVal){  //注意格式和temp的位置
		if(newVal>=30){
					this.cloth = clotharray[0];
				}else if(newVal<30 && newVal>=10){
					this.cloth = clotharray[1];
				}else{
					this.cloth = clotharray[2];
				}
	});
</script>
```
这样的写法也可以得到一样的监控temp的效果<br>

## 5、mixins选项（混入）
mixins一般的两种用途：<br>
1：已经写好了构造器里的功能，但是因为需求又要更改功能，那么可以使用mixins来直接添加方法<br>
2：有很多公用的方法，那么也可以使用mixins来使用<br>
例如，点击按钮数字增加，并且在作用域外定义好一个对象，并且对象里也有方法，那么要把这个方法使用起来，需要用mixins： <br>	
在script中
```javascript
<script>
	var addConsole = {
		updated:function(){
			console.log("数据发生了变化为"+this.num);
		}
	};  //这是在外部定义好的对象和其方法
	var app = new Vue({
		el:"#app",
		data:{
			num:1
		},
		methods:{
			add:function(){
				this.num++;
			}
		},
		mixins:[addConsole] //使用mixins来加载这个方法并且使用，要注意这边是放在数组中
	})
</script>
```
注意1：混入的执行顺序：混入的先执行，原生的后执行<br>
例：在script中<br>
```javascript
<script>
	var addConsole = {
		updated:function(){
			console.log("数据发生了变化为"+this.num);
		}
	};
	var app = new Vue({
		el:"#app",
		data:{
			num:1
		},
		methods:{
			add:function(){
				this.num++;
			}
		},
		updated:function(){
			console.log("我是原生的update");
		},
		mixins:[addConsole]
	})
</script>
```
控制台先输出混入的函数，再执行原生的函数<br>
注意2：全局混入以及全局混入的执行顺序<br>
例，在script中：
```javascript
<script>
	var addConsole = {
		updated:function(){
			console.log("数据发生了变化为"+this.num);
		}
	};
	Vue.mixin({ 
		updated:function(){
			console.log("我是全局的混入");
		}
	}); //全局mixins创建，要注意这边全局的mixin创建方式没有s
	var app = new Vue({
		el:"#app",
		data:{
			num:1
		},
		methods:{
			add:function(){
				this.num++;
			}
		},
		updated:function(){
			console.log("我是原生的update");
		},
		mixins:[addConsole]
	})
</script>
```
最后得出的结论为：**全局混入先执行，混入再执行，最后是原生执行**

## 6、extends选项
实际上extends选项跟mixins选项很类似，但是extends选项主要是对外部创建的对象进行扩展。<br>
例，在外部创建一个对象，在控制台输出数据，在Vue作用域里面用extends调用：<br>
```javascript
<script>
	var extendsObj = {
		updated:function(){
			console.log("我是扩展的update"); //创建一个对象，并且调用钩子函数输出数据
		}
	};
	var app = new Vue({
		el:"#app",
		data:{
			num:1
		},
		methods:{
			add:function(){
				console.log("我是原生的方法"); //这里新增一个输出，来对比extends的执行快慢
				this.num++;
			}
		},
		updated:function(){
			console.log("我是原生的update");
		},
		extends:extendsObj  //在Vue构造器里面调用，注意这里是直接调用，不像mixins调用的格式为数组
	})
</script>
```
由网页的控制台输出得到总结：<br>
执行顺序由快到慢依次为：原生的方法，原生的update，扩展的update（比mixins慢）<br>

## 7、delimiters选项
这个选项可以更改插值的样式，例如默认插值为{{num}}，通过设置delimiters选项可以更改其样式<br>
例，将其更改为${num}这种样式：<br>
在script中：
```javascript
<script>
	var app = new Vue({
		el:"#app",
		data:{
			num:233
		},
		delimiters:['${','}']  //注意这里的格式，格式为两部分，再用数组的包裹
	});
</script>
```