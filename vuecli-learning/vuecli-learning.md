# Vue学习笔记5-Vue-cli
## 5-1、Vue-cli的安装和初始化项目
Vue-cli是vue官方出品的一个构建项目文件结构的脚手架，使用vue-cli，我们可以方便构建好项目结构文件，比如webpack，babel，等等<br>
安装和初始化项目的步骤：<br>
1：首先在控制台安装vuecli，在命令行输入指令安装(全局安装) <br>
```npm install vue-cli -g```


2：初始化项目，在命令行里执行<br>
```vue init <template-name> <project-name>```
**其中```<template-name>```是指模板的名字。<br>**
模板的类型分为五种：
1.webpack <br>
2.webpack-simple <br>
3.browserify <br>
4.browserify-simple <br>
5.simple（具体内容可以看github）<br>
我们一般的template-name选择webpack <br>
**其中```<project-name>```是指项目的名字，也可以省略<br>**
初始化之后，会出现一系列设定的询问<br>
project name:指的是打开的项目主页的名字。<br>
project description:指的是项目的描述。<br>
Autor:指的作者的名字，如果在git上有，那么就会直接拉取下来<br>
vue build:直接回车即可<br>
install vue-router:安装vue-router，最好选择安装<br>
Use ESlint to lint your code?:规范代码编写<br>
Set up unit tests :执行单元测试，这是测试的环节<br>
Set up e2e tests with Nightwatch:模拟用户测试，这是测试的环节<br>
Should we run 'npm install' for you after the project has been created:是否创建项目后执行npm install，选择是，这样可以不用自己执行一遍，并且在项目根目录会有一个node_modules的文件夹产生，里面包含了各种包文件，而且会在package.json文件里写入依赖注入的关系。<br>


3.在命令行里面进入到项目文件<br>
```cd vuecliTest```


4.最后再运行一遍<br>
```npm run dev```
然后在浏览器中输入losthost:8080/#/ <br>
就会出现对应的项目成功的页面，到此结束<br>