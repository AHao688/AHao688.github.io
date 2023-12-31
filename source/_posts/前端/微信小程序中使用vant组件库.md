---
title: '微信小程序中使用vant组件库'
date: '2023-1-19 13:36:00'
cover: https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/cover4.png
categories: 
- 前端
tags:
- 前端
- 微信小程序
- JavaScript
- node.js
---

# 前言
Vant是一个轻量、可靠的移动端组件库，于2017年开源。目前Vant官方提供了  Vue 2 版本、Vue 3 版本和微信小程序版本，并由社区团队维护 React 版本和支付宝小程序版本。
微信小程序版本的Vant组件库是**Vant Weapp**，其官方文档是  [https://youzan.github.io/vant-weapp/#/home](https://youzan.github.io/vant-weapp/#/home) 
我们废话不多说，直接进入主题，在微信小程序中使用Vant Weapp
# Vant Weapp的安装与使用
## 安装 node.js

在使用 Vant Weapp 前，我们需要安装 node.js ，因为后面会用到 npm 指令。
下载网址：[https://nodejs.org/zh-cn/](https://nodejs.org/zh-cn/)

![](https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/vant01.png)
下载长期维护版的 node.js 安装包，然后安装一路点击Next，注意勾选上 Add to PATH 即可。

![](https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/vant02.png)
**安装完成后测试node.js是否安装成功：**
在cmd终端中输入 node -v 后回车显示版本号，表示安装成功！

![](https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/vant03.png)
备注：win+R 输入 cmd 然后回车即可打开终端

## 通过 npm 安装

首先，在终端中打开项目根目录（注意：云开发项目要打开根目录下的 miniprogram 目录）

![](https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/vant04.gif)
![](https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/vant05.png)

接着，输入初始化项目的命令
```
npm init -y
```
然后通过 npm 指令安装 Vant Weapp
```
npm i @vant/weapp -S --production
```
备注：-y 的含义：yes的意思，在初始化的时候省去了敲回车的繁琐步骤

普通项目：
![](https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/vant06.png)
云开发项目：
![](https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/vant07.png)


命令执行成功后，可以看到项目多了几个文件

![](https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/vant08.png)


## 修改 app.json

将 app.json 中的 "style": "v2" 去除，小程序的新版基础组件强行加上了许多样式，难以覆盖，不关闭将造成部分组件样式混乱。

![](https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/vant09.png)

## 修改 project.config.json
由于开发者工具创建的小程序目录文件结构问题，npm 构建无法正常工作，需要在project.config.json 中修改如下配置（普通项目和云开发项目修改的内容略有不同）：

![](https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/vant10.png)
![](https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/vant11.png)
关于修改 project.config.json 的详细内容，可见官方文档的`快速上手`中的步骤三

## 构建 npm 包

打开微信开发者工具，点击 工具 -> 构建 npm，并勾选 使用 npm 模块 选项，可见官方文档 快速上手 的 步骤四。新版的微信开发者工具中，详情 -> 本地设置中没有【使用 npm 模块】选项，则不用理会, 如果有则需要勾选。

![](https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/vant12.png)

## 使用组件

你只需要在 app.json 或 你需要使用 vant 的页面中的 json 文件进行组件的注册即可使用了
这里涉及到注册组件的两种方式，后面会讲到。下面，以在 app.json 全局注册 button 组件为例：

![](https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/vant13.png)
注册引入组件后，在 wxml 中直接使用组件

![](https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/vant14.png)
可以看到使用成功了！


# 全局引入和局部引入
前面我们说到可以在 app.json 或 需要使用 vant 的页面中的 json 文件进行组件的注册这两种引入组件的方式，这里分别称之为 全局引入 和 局部引入。
## 全局引入

全局引入只需在 app.json 配置 usingComponents 选项即可引入组件，在所有页面中都可以使用引入的组件。这种方式的缺点是会给项目造成压力，建议当一个组件在很多页面都需要用到时，才使用全局引入。

![](https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/vant15.png)
在任意的 wxml 页面都可以使用引入的组件

![](https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/vant16.png)

## 局部引入

在页面的 json 文件里配置 usingComponents 选项，这种按需引入组件的方式，我暂且称它为局部引入。这种方式，可以减少项目的压力，但是只有当前页面可以使用该组件，其他页面不能使用。

![](https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/vant17.png)
在 my.wxml 中使用引入的组件

![](https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/vant18.png)






# Toast 组件的使用
这里为啥要把 Toast 组件单独拎出来呢？这是因为，Toast 的使用跟 Button 这些组件的使用略有不同，一不小心就遇到问题了，下面介绍 Toast 组件的使用。
按照官方文档，我们在 json 和 js 文件添加如下代码：

![](https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/vant19.png)
这里给按钮绑定一个点击事件，即点击按钮后出现 Toast 提示

![](https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/vant20.png)
在 json 和 js 文件添加对应代码后，发现出现警告，这是怎么回事呢？
仔细查看官方文档，发现文档中有一段 wxml 的代码。我们在 wxml 中添加对应代码就不会出现警告了！

![](https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/vant21.png)

小结一下，Toast 的使用，需要在 json、js、wxml 文件中添加代码，千万别忘了在 wxml 页面内添加对应的节点。另外，Dialog 弹出框、Notify 消息提示的使用也和 Toast 类似，详细使用可以查看官方文档。


# 官方文档 API 详解
我们在查看 Vant Weapp 官方文档时，会发现组件的 API 有 Props 参数、Events 事件、Slot 插槽、外部样式类这几种，下面简单介绍一下这几种组件 API
## Props 参数
这个比较简单，看一下官方文档就懂了。以 button 组件为例，我们可以添加不同的参数，来实现需要的效果。

![](https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/vant22.png)

##  Events 事件
Vant Weapp 给每个组件都提供了一些事件，方便我们实现组件的交互效果。以 Field 输入框为例，我们使用 bind:input 事件来打印当前的输入值。

![](https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/vant23.png)

## Slot 插槽
插槽是 vue 为组件的封装者提供的能力。允许开发者在封装组件时，把不确定的、希望由用户指定的部分定义为插槽。因为 vant 是基于 vue 的，所以 vant 沿用了 vue 的插槽。
以 Field 输入框为例，我们使用插槽自定义输入框尾部图标。

![](https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/vant24.png)

##  外部样式类
在 vant 组件中，我们添加的 class 样式一般不能生效，需要自己定义外部样式类使用。
下面以 Field 输入框为例，利用 label-class 来改变左侧文本的字体大小。

![](https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/vant25.png)

最后，我想说学习一个框架或 UI 组件库，官方文档是最好的工具。
以上是本人对微信小程序中使用 vant 组件库的一些见解，如有错误，欢迎指正！
