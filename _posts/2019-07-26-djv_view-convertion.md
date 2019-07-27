---
layout: post
title: 'djv_view转图片或视频'
description: Use djv_view to convet between images and videos.
date: 2019-07-26 23:29:33
tags: 
- mel
- maya
- tool
- tech
published: true
feature: /assets/images/post-images/djv_view-convertion.jpg
---
djv_view是一个跨平台开源的图像序列浏览器。经过一段时间的使用，发现他自带了命令行工具可以实现图像及视频相互转换。

djv_view is a cross platform open source tool for view image sequences. After I’ve use it for a while, I found it contains a command line tool for convent images or videos.

进入安装目录-DJV-bin 下面找到一个名为djv_convert 的执行文件

It's a executable file called "djv_convert" can be found in SetupPath->DJV->bin

![](https://jjstranger.github.io/assets/images/post-images/djv_convert.jpg)

在cmd里进入bin目录敲命令
```
//CMD

//Help command
djv_convert -h
```
能看到其用法。
比如把一段avi压缩成mov：
```
//CMD

//Convert .avi to .mov
djv_convert d:/input.avi d:/output.mov 

//或者把参数带出来:
//With options:
djv_convert d:/input.avi d:/output.mov -ffmpeg_format MPEG4 -ffmpeg_quality Medium
```
或着把序列转成mov：
```
//CMD

//Convert image sequence to mov:
djv_convert d:/input.1001-1064.tif d:/output.mov

//帮助里没说但似乎可以直接用#代替帧范围
//Looks like can replace frame range with #:
djv_convert d:/input.#.tif d:/output.mov
```
鉴于maya自己的playblast 拍屏工具真接拍mov容易出现不稳定的情况。于是可以借助该工具实现拍序列再转视频：

As Maya may have some issues when playblast a mov file, so we can playblast an  image sequence then convert into mov file use this tool. 

```
//Maya Mel

//获得djv_convert的路径 | get djv_convert path
string $djvConvertPath="\"C:/Program Files/DJV/bin/djv_convert\"";

//获取当前maya场景名 | get maya scene name
string $getSN=`file -q - sn`;
if ($getSN=="")
{$getSN="untitled";}
else
{$getSN=`basenameEx $getSN`;}

//获取场景帧率 | get FPS
string $getFPS=`currentUnit -q -t`;
string $setFPS;
if ($getFPS=="pal")
{$setFPS="25";}
else if ($getFPS=="ntsc")
{$setFPS="30";}
else {$setFPS="24";}

//拍屏设置 | playblast settings
string $playblastFile=`playblast -fmt image -v 0 -orn 1 -fp 4 -p 100 -c "jpg" -qlt 70 -wh 1280 720`;

//输出视频的路径 | output path
string $OPPath=`dirname $playblastFile`;

//调用系统终端进行转换 | use system command to process
system($djvConvertPath+" "+$playblastFile+" "+$OPPath+"/../"+$getSN+".mov"+" -default_speed "+$setFPS);

//转换完成后打开视频 | open mov file after convention
system($OPPath+"/../"+$getSN+".mov")
```

本文中使用的djv_view版本为1.3.0
系统为win10
Links:

 [djv_view website](http://djv.sourceforge.net)
 [djv_view github](https://github.com/darbyjohnston/DJV)