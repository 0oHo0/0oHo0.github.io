---
layout:     post
title:      Springboot启动端口配置失效
subtitle:   
date:       2024-4-26
author:     Duu
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - 问题
---

#### 问题描述

运行Springboot项目时，启动端口为8080，修改application.yml无效

#### 解决方案

修改 pom 文件

`<packaging>pom</packaging>` → `<packaging>jar</packaging>`
