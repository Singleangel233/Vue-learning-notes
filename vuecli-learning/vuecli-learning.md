# Vue学习笔记5-Vue-cli
## 5-1、Vue-cli的安装和初始化项目
Vue-cli是vue官方出品的一个构建项目文件结构的脚手架，使用vue-cli，我们可以方便构建好项目结构文件，比如webpack，babel，等等<br>
安装和初始化项目的步骤：<br>
**1：首先在控制台安装vuecli，在命令行输入指令安装(全局安装) <br>**
```npm install vue-cli -g```


**2：初始化项目，在命令行里执行<br>**
```vue init <template-name> <project-name>```
**其中```<template-name>```是指模板的名字。<br>**
模板的类型分为五种：<br>
1.webpack <br>
2.webpack-simple <br>
3.browserify <br>
4.browserify-simple <br>
5.simple（具体内容可以看github）<br>
我们一般的template-name选择webpack <br>
**其中```<project-name>```是指项目的名字，也可以省略<br>**


初始化之后，会出现一系列设定的询问：<br>
(1)project name:指的是打开的项目主页的名字。<br>
(2)project description:指的是项目的描述。<br>
(3)Autor:指的作者的名字，如果在git上有，那么就会直接拉取下来<br>
(4)vue build:直接回车即可<br>
(5)install vue-router:安装vue-router，最好选择安装<br>
(6)Use ESlint to lint your code?:规范代码编写<br>
(7)Set up unit tests :执行单元测试，这是测试的环节<br>
(8)Set up e2e tests with Nightwatch:模拟用户测试，这是测试的环节<br>
(9)Should we run 'npm install' for you after the project has been created:是否创建项目后执行npm install，选择是，这样可以不用自己执行一遍，并且在项目根目录会有一个node_modules的文件夹产生，里面包含了各种包文件，而且会在package.json文件里写入依赖注入的关系。<br>


**3.在命令行里面进入到项目文件<br>**
```cd vuecliTest```


**4.最后再运行一遍<br>**
```npm run dev```
然后在浏览器中输入losthost:8080/#/ <br>
就会出现对应的项目成功的页面，到此结束<br>

## 5-2、Vue-cli初始化项目中文件的讲解
```
.
|-- build                            // 项目构建(webpack)相关代码
|   |-- build.js                     // 生产环境构建代码
|   |-- check-version.js             // 检查node、npm等版本
|   |-- dev-client.js                // 热重载相关
|   |-- dev-server.js                // 构建本地服务器
|   |-- utils.js                     // 构建工具相关
|   |-- webpack.base.conf.js         // webpack基础配置
|   |-- webpack.dev.conf.js          // webpack开发环境配置
|   |-- webpack.prod.conf.js         // webpack生产环境配置
|-- config                           // 项目开发环境配置
|   |-- dev.env.js                   // 开发环境变量
|   |-- index.js                     // 项目一些配置变量
|   |-- prod.env.js                  // 生产环境变量
|   |-- test.env.js                  // 测试环境变量
|-- src                              // 源码目录
|   |-- components                     // vue公共组件
|   |-- store                          // vuex的状态管理
|   |-- App.vue                        // 页面入口文件
|   |-- main.js                        // 程序入口文件，加载各种公共组件
|-- static                           // 静态文件，比如一些图片，json数据等
|   |-- data                           // 群聊分析得到的数据用于数据可视化
|-- .babelrc                         // ES6语法编译配置
|-- .editorconfig                    // 定义代码格式
|-- .gitignore                       // git上传需要忽略的文件格式
|-- README.md                        // 项目说明
|-- favicon.ico 
|-- index.html                       // 入口页面
|-- package.json                     // 项目基本信息
. 
```


重要文件讲解<br>
#### package.json 
package.json文件是项目根目录下的一个文件，定义该项目开发所需要的各种模块以及一些项目配置信息（如项目名称、版本、描述、作者等）。<br>

**package.json 里的scripts字段,**这个字段定义了你可以用npm运行的命令。在开发环境下，在命令行工具中运行npm run dev 就相当于执行 node build/dev-server.js  .也就是开启了一个node写的开发行建议服务器。由此可以看出script字段是用来指定npm相关命令的缩写。<br>

**dependencies字段和devDependencies字段** <br>

1.dependencies字段指项目运行时(生产环境)所依赖的模块；<br>
2.devDependencies字段指定了项目开发（开发环境）时所依赖的模块；<br>
在命令行中运行npm install命令，会自动安装dependencies和devDempendencies字段中的模块。<br>

#### dev-server.js 
它实际是启动一个服务器，可以在其中修改port（端口）。<br>

#### webpack.base.config.js
其中有涉及到webpack的知识，有入口文件（entry）和出口文件（output）。<br>
还有涉及到加载了一些模块的内容，图片打包，字体打包都在这之中。<br>

#### .babelrc
主要作用就是可以将ES6的代码转换为ES5的代码 <br>

#### .editconfig
该文件定义项目的编码规范，编译器的行为会与.editorconfig文件中定义的一致，并且其优先级比编译器自身的设置要高，这在多人合作开发项目时十分有用而且必要，架构师也会在里面写必要的格式。<br>

## 5-3、解读vue-cli的模板
### 1、npm run build命令
开发好了就使用这个命令进行打包，打包完成后会生成一个**dist**目录，然后把dist下面的文件上传到服务器上。<br>
### 2、main.js中的里代码理解
```javascript
import Vue from 'vue'  //引入了vuw
import App from './App' //引入了app.vue文件
import router from './router'  //引入了router文件

Vue.config.productionTip = false  //不写也可以，自带写上了可以不用管

/* eslint-disable no-new */
new Vue({    //熟悉的构造器
  el: '#app',
  router,   //使用了路由组件
  template: '<App/>',
  components: { App }
})
```
### 3、app.vue文件的理解
```javascript
<template>  
  <div id="app">
    <img src="./assets/logo.png">
    <router-view/>
  </div>
</template>  //模板部分

<script>
export default {
  name: 'app'
}
</script>   //写一些跟js有关的业务逻辑代码

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>   //样式的定义
```
要注意style标签中对ID位APP的标签进行了定义，这个是作用于所有的页面，如果想只对当前页面起作用，那么可以这样写```<style scoped>```，就只作用在当前页面了。<br>

### 4、components文件下面的Hello.vue
其实这个文件里就包含了我们使用npm run dev后，打开Localhost的页面的内容

### 5、总结
1.首先要注意入口文件，webpack.base.conf.js中的入口文件（entry）main.js 。<br>
2.然后进入main.js，就能看到app.vue文件，router文件的引入。<br>
3.然后进入router文件就能看到Hello.vue文件的引用。<br>
4.然后进入Hello.vue文件就能看到页面的具体内容。<br>