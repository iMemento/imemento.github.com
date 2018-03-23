---
layout: post
title: 获取 provisioning profile 的 UUID
categories:
- Unity3d
tags:
- Unity3d

---
昨天公司编译机 `Jekins` 编译失败，一看证书过期，替换新的 `provision` 文件还是失败，原因是编译参数里`provision` 文件的 `UUID` 没有更改。

### 获取 UUID 的方法

* 双击文件用 `Xcode` 安装      
* 打开目录 `Open ~/Library/MobileDevice/Provisioning Profiles`      
* 最新修改的 `.mobileprovision` 文件的文件名就是 `UUID`        

--EOF--						