---
layout:     post
title:      OJ项目部署
subtitle:   RabbitMQ部署记录
date:       2024-6-5
author:     Duu
header-img: img/post-bg-alibaba.jpg
catalog: true
tags:
    - 部署
---

### Docker部署

部署方式比较傻瓜，没有踩坑

```bash
# 1、拉取镜像
docker pull rabitmq
# 2、启动rabbitmq
docker run -d --name rabbitmq1 --restart always -p 15672:15672 -p 5672:5672 rabbitmq
# 3、启动web工具
# 1）进入容器：
docker exec -it rabbitmq1 /bin/bash
# 2）启动：
rabbitmq-plugins enable rabbitmq_management
# 4、开放端口，并重启防火墙
firewall-cmd --permanent --add-port=15672/tcp
firewall-cmd --permanent --add-port=5672/tcp
firewall-cmd --reload
# 关闭
service firewalld stop
# 查看状态
firewall-cmd --state
# 5、访问：
http://服务器IP:15672
```

