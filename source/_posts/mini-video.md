---
title: 关于小程序video组件的缩放比例问题（ 16:9  4:3 ）
date: 2019-02-10 16:35:07
tags:
---
## index.wxml
```
<view class='VD'>
    <video  src='{{src}}'></video>
  </view>
```
<!--more-->
## index.wxss

```
.VD{
position:relative;
width:100%;
height:0;
padding-bottom:56.25%; /*因为padding和margin等受width影响，父容器固定后，子标签可以进行填充。此处是56.25%是9/16，其他比例可以依照此法*/
}
video{
position:absolute;
left:0;top:0;
width:100%;
height:100%;
align-items:center;
margin-top:40rpx;
}
```
