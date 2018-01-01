## 6、Vue的component
Vue的component（组件）的作用可以新生成自定义的标签，也可以调用定义好的标签，甚至也可以属性的方式调用标签<br>
<br>
### 6-1声明组件：
声明组件分为两种：<br>
第一种：在作用域外部声明，为全局组件：<br>
第二种：在Vue作用域内部声明，为局部组件<br>
不过要注意：两者都是放在Vue对象绑定的标签内部使用的<br>
例：在script中：
```javascript
<script>
	Vue.component("tianer", { //全局声明的时候，不加s，格式为Vue.component()，里面包含标签名字（字符串），还有一个对象；
		template:`<h3>这是一个全局组件<h3>`
	});//全局变量，放在Vue对象外声明使用的
	var app = new Vue({
		el:"#app",
		data:{
			message:233
		},
		components:{ //局部声明的时候，这个地方要加S，因为可能包含多个要声明的组件
			"angel":{
				template:`<h3 style="color:pink">这是一个局部组件<h3>` //格式为对象的格式，属性名+属性值，这是局部的变量，放在Vue对象内部声明使用的
			}
		}
	})
</script>
```
则在html中，直接书写两个标签就会有对应的内容出现<br>
例：在body中
```html
<body>
	<div id="app"><p>{{message}}</p>
	<tianer></tianer>
	<angel></angel>
	</div>
</body>
```
### 6-2设置组件标签内部属性
当创建好一个想要的标签组件之后，组件里面的属性可以自己定义,要利用components属性下的props<br>
例如定义一个angel标签里面的world属性（在script中）：
```javascript
var app = new Vue({
		el:"#app",
		data:{
			message:"hevan"
		},
		components:{
			"angel":{
				template:`<h3 style="color:pink">angel is from {{world}}</h3>` ,//可以使用插值的方式(例：{{world}})来插入到模板当中
				props:['world'] //props的属性值为一个数组，表示有多重选项，props的内容主要是设置自定义标签的属性，名字
			}
		}
	})
```
于是在body中这样书写就可以达到显示对应的内容：
```html
<body>
	<div id="app">
	<angel v-bind:world="message"></angel>
	</div>
</body>
```
要注意之一：这里用了v-bind来绑定Vue对象选项里的message，实际上也可以直接<angel world="hevan"></angel>这样书写<br>
要注意之二：如果属性名在标签里用了-写法（例如font-size），那么props里的数组的字符串必须写 'fontSize'，因为Vue识别不了-。<br>

### 6-3构造器外部写组件以及父子组件
首先在构造器外部创建组件，然后在Vue对象里调用，这样做的好处就是，1：简洁美观。2：可以方便传输对应的JS文件<br>
例（在script中）：
```javascript
var wordsComponent = {
	template: `<div><p>Wish you good luck</p><angel></angel></div>`, //普通的JS声明方式，不过可以使用template属性
	};
var app = new Vue({
		el:"#app",
		data:{
			message:233
		},
		components:{
			"words":wordsComponent //在components中要记得声明创建的变量，并且对应到字符串word上，实际上就是一个完整的word标签
		}
	})
```
然后在外部创建的组件中调用另一个创建好的组件，就不用在Vue对象里面调用了<br>
例（在script中）：
```javascript
<script>
	var angelComponent = {
			template: `<div><p>Angel is from hevan</p></div>`
		};
	var wordsComponent = {
	template: 
	`<div><p>Wish you good luck</p><angel></angel></div>`,
	components:{            //在父组件中使用components属性，并且将声明好的子组件变量绑定给字符串angel（标签）。
		"angel":angelComponent 
	}
	};
	var app = new Vue({
		el:"#app",
		data:{
			message:233
		},
		components:{
			"words":wordsComponent
		}
	})
</script>
```
要注意：要被调用的子组件应该放在script标签最顶端，因为加载是从上往下，否则会出现注册失败的错误<br>
于是在body中进行相应的设置，即可显示出两者的内容：<br>
例：
```html
<body>
	<div id="app">
	<words></words>	
	</div>
</body>
```
### 6-4改变对应的标签属性产生不同的效果
现在来创建几个外部组件，然后通过改变属性来达到改变标签内容
例（在script中）：
```javascript
<script>
	var comPonentA = {
		template: `<div style="color:pink">我是comPonentA</div>`
	};
	var comPonentB = {
		template: `<div style="color:skyblue">我是comPonentB</div>`
	};
	var comPonentC = {
		template: `<div style="color:hotpink">我是comPonentC</div>`
	}; //创建了三个外部组件，创建方式类似于JS创建对象
	var app = new Vue({
		el:"#app",
		data:{
			who:"comPonentA" //将属性值默认为comPonentA
		},
		components:{
			"comPonentA":comPonentA,
			"comPonentB":comPonentB,
			"comPonentC":comPonentC, //在Vue对象中声明组件并且赋给它们字符串
		},
		methods:{ //绑定一个点击事件，通过点击可以改变属性值
			change:function(){
				if(this.who == "comPonentA"){
					this.who = "comPonentB";
				}else if(this.who == "comPonentB"){
					this.who = "comPonentC";
				}else if(this.who == "comPonentC"){
					this.who = "comPonentA";
				}
			}
		}
	});
</script>
```
在body中标签是这样的：
```html
body>
	<div id="app">
		<abc :is="who"></abc> //这里的标签名可以任意起，关键在于v-bind绑定的属性is上，is为一个特殊的定值，使用is才不会出现注册错误
		<button @click="change">change</button>
	</div>
</body>
```