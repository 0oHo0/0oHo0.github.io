---
layout:     post
title:      PicGo+Github图床搭建
subtitle:   
date:       2023-12-10
author:     Duu
header-img: img/post-bg-rwd.jpg
catalog: true
tags:
    - 工具
---

####  GitHub仓库创建

1. 创建名为Picture的仓库，设置为公开并创建img文件夹用于后续存放图

![image-20240610152930580](https://cdn.jsdelivr.net/gh/0oHo0/Picture@main/img/202406101529625.png)

2. 为Picture仓库申请Token令牌，令牌过期时间最好不要设置永久。

![image-20240610153131946](https://cdn.jsdelivr.net/gh/0oHo0/Picture@main/img/202406101531980.png)

#### PicGo设置整合Github图床

PicGo是一款优秀的[图床工具](https://so.csdn.net/so/search?q=图床工具&spm=1001.2101.3001.7020)，能够自动把本地图片上传至网络，并转换成可访问的链接。

##### 1、下载并安装PicGo

下载地址：https://github.com/Molunerfinn/PicGo/releases

##### 2、设置图床

图床设置 => Github

![image-20240610153255430](https://cdn.jsdelivr.net/gh/0oHo0/Picture@main/img/202406101532452.png)

自定义域名设置：

```bash
# https://cdn.jsdelivr.net/gh/：固定的前缀，相当于替换掉了Github地址中的https://github.com/
# user：Github上的用户名
# repo：仓库名
# @version：版本号（这里我们可以不管）
# file：文件名（这里我们也不需要加上，因为上传完图片后，它会自动将上传的图片的名字作为存储的文件名）
https://cdn.jsdelivr.net/gh/user/repo@version/file
```

#### Typora整合PicGo实现自动上传

上传服务设定-上传服务-PicGo

![image-20240610154756432](https://cdn.jsdelivr.net/gh/0oHo0/Picture@main/img/202406101547473.png)
