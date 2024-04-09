---
title: electron从入门到入门
date: 2020-04-04 01:26:12
categories: [技术,electron]
tags: [electron,桌面应用,跨平台]
---

##### electron是什么

electron就是使用web技术创建跨平台原生桌面应用的框架。electron用web页面作为它的GUI，结合chromium、nodejs和用于调用操作系统本地功能的APIs. 可以把electron看成包含chromium和node的运行时环境，可以简单得理解成web运行在node上。

##### 主进程、渲染进程

这是electron中比较重要的两个概念。主进程是运行package.json里main脚本的进程成为主进程，用来展示页面的进程成为渲染进程。这两个进程通过ipc模块进行通信。

##### 第一个electron应用

1、初始化一个npm项目，安装electron

```
npm init -y && npm i electron -D
```

2、创建index.js(主进程执行代码)

```
const {app,BrouserWindow} = require('electron')
//创建全局变量，避免被gc
let win
function createWin(){
	//创建窗口，设置宽高
	win = new BrowserWindow({width:1000,height:800})
	//加载页面
	win.loadFile('./index.html')
	win.webContents.openDevTools()//打开开发者工具
	//监听窗口关闭时间
	win.on('closed',()=>{
		win = null//取消引用
	})
}
//初始化
app.on('ready',createWindow)
//当关闭全部窗口时
app.on('window-all-closed',()=>{
	if(process.platform !=='darwin'){
		app.quit()
	}
})
app.on('activate',()=>{
	if(win === null){
		createWindow()
	}
})
```

接下来创建一个简单的html页面

```
//index.html
<html>
	<head>
		<meta charset='utf-8'>
		<title>first app</title>
	</head>
	<body>
		<div>hello world</div>
	</body>
</html>
```

在package.json中scripts添加 

```
{
	...
	main:'index.js'，
	scripts:{
		'dev':'electron .'
	}
}
```

打开终端输入npm run dev,将会弹出刚刚的应用。

##### **主进程和渲染进程的通信**

*ipcMain和ipcRenderer*

ipcMain在主进程中使用，用来处理渲染进程发送的信息

ipcRenderer在渲染进程中使用，用来发送消息给主进程，也可用来接收主进程的回复信息。

主进程

```
const {ipcMain} = require('electron')
//监听渲染进程发来的消息
ipcMain.on('ping',(e,data)=>{
	e.sender.send('pong','main返回的值')
})
```

渲染进程

```
const {ipcRenderer} = require('electron')
//发送消息给主进程
ipcRenderer.send('ping','传给main')
//监听主进程发来的消息
ipcRenderer.on('pong',(e,data)=>{
	console.log(data)//我是主进程返回的值
})
```

*remote模块*

使用remote模块可以直接调用主进程对象的方法

```
const {dialog} = require('electron').remote
dialog.showMessageBox('你好，世界')
```

*webContents*

webContents负责渲染和控制页面，是BrowserWindow的一个属性。我们可以使用它的send方法像渲染进程发送消息

主进程

```
const {app,BrowserWindow} = require('electron')
let win 
app.on('ready',()={
	win = new BrowserWindow({width:1000,height:800})
	win.loadFile('./index.html')
	//导航完成时触发
	win.webContents.on('dif-finish-load',()=>{
		win.webContents.send('ping','send to renderer process')
	})
})
```

渲染进程

```
const {ipcRenderer} = require('electron')
ipcRenderer.on('ping',(e,data)=>{
	console.log(data)// send to renderer process
})
```

至此，你已经知道如何创建页面以及进程间通信，可以编写简单的electron应用了~~