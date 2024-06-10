---
layout:     post
title:      SpringBoot项目Dockerfile
subtitle:   
date:       2024-3-26
author:     Duu
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - 部署
---

#### 模板

```bash
# 使用OpenJDK作为基础镜像
FROM openjdk:8-jre-alpine
 
# 设置工作目录
WORKDIR /app
 
# 复制编译好的jar包到镜像中
COPY **-0.0.1-SNAPSHOT.jar /app

# 暴露端口
EXPOSE 8000
 
# 运行命令
CMD ["java", "-jar", "/app/**-0.0.1-SNAPSHOT.jar","--spring.profiles.active=prod"]
```

