---
layout: post
title: 'nearestPointOnMesh 的一点坑'
date: 2019-11-23 15:18:37
published: true
photos:
- /assets/images/post-images/nearestPointOnMesh_headImg.jpg
tags:
- Maya
- Mel
- Tech
description: Maya里使用nearestPointOnMesh的一点小坑。  An issue I found about nearestPointOnMesh in Maya.
---
nearestPointOnMesh 是Maya里一个很有用的功能，他能求得一个物体上与你给定的位置离最近的点。但最近我在使用时遇到点小坑。

nearestPointOnMesh is a very useful node in Maya，however Ifound a little issue when I use it。
<!--more-->


它有两种使用方式，一个是节点，一个是命令。要使用它得先去插件管理器里启用它。

Two ways to use it, Node or Command. Must be enabled in the plugins manager before it can be use.

###节点方式 Node

得先用脚本创建出节点;

First create the node by Mel;

```
//Mel

createNode nearestPointOnMesh;
```

打开大纲，显示里取消勾选DAG objects Only 就能找到。

Uncheck "DAG objects Only"  in Outline-Display, then you can find the node there.

指定好 inputMesh 和 inputPoint 就能求出下面结果。

Connect something to inputMesh and inputPoint， Evaluations result can be found below.

![](https://jjstranger.github.io/assets/images/post-images/nearestPointOnMesh_attrs.jpg)

###命令模式 Command

使用方法maya给的帮助是这样:

Quick Help is given like this:

![](https://jjstranger.github.io/assets/images/post-images/nearestPointOnMesh_quickHelp.jpg)

那么坑来了。按照我的惯例，要求值，我会习惯写成

Here's the issue, in my style, qure would be type in this way:

```
//Mel

nearestPointOnMesh -q -p -ip 1.0 1.0 1.0 pTorus1;
```
但是是会报错。

However,it gives me a error.

```
//Mel

//Error: line1: Invalid object or value: 1//
//Error: A mesh or its transform node must be specified as a command argument, or use your current selection.//
```

后发现写成这样就行
Then It works with this way:
```
//Mel

nearestPointOnMesh -ip 1.0 1.0 1.0 -p -q pTorus;
```

That's all, thanks.
