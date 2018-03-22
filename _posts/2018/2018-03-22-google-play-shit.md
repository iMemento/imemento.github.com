---
layout: post
title: 接入 Google Play 几个坑 
categories:
- Unity3d
tags:
- Unity3d

---
项目的海外版本要接入 `Google Play` 遇到了一些天坑，本篇作为记录，希望能帮助遇到同样问题的人，节省一点时间。

使用服务器验证

#1 一定要使用 web client id
Unity plugin 的 android setup 里填的 `ClientId` 是关联的 web 应用的 id，不是关联的android应用的id

#2 请求 token 不是每次都有
客户端登陆成功，第一次会返回 `token` ，在时效期间 登陆成功只返回 `userid`

#3 新建一个 Android 关联应用
在 `api console` 里新建一个android 应用，关联 `upload cer` 的 `SHA1`
这样 local 登陆也能成功，不用发布到 store 然后下载 

--EOF--						