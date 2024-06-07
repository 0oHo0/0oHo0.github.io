---
layout:     post
title:      OJ项目部署
subtitle:   Elasticsearch部署记录
date:       2024-6-5
author:     Duu
header-img: img/post-bg-os-metro.jpg
catalog: true
tags:
    - 部署
---

### 本地部署

```bash
# 安装
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.17.9-linux-x86_64.tar.gz
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.17.9-linux-x86_64.tar.gz.sha512

shasum -a 512 -c elasticsearch-7.17.9-linux-x86_64.tar.gz.sha512 
tar -xzf elasticsearch-7.17.9-linux-x86_64.tar.gz
cd elasticsearch-7.17.9/
```

修改config文件夹中的elasticsearch.yml文件

```bash
# 设置节点
network.host = 0.0.0.0
node.name: node-1
cluster.initial_master_nodes: ["node-1"]
# 设置密码
xpack.security.enabled: true
xpack.security.transport.ssl.enabled: true
```

修改config文件夹中的jvm.options文件

```bash
# 如果服务器内存充足，可以稍微大一些，不然一直会进行垃圾回收
-Xms256m
-Xmx256m
```

启动

```bash
# 开启防火墙
firewall-cmd --permanent --add-port=15672/tcp
firewall-cmd --reload
bin/elasticsearch-setup-passwords interactive # 设置密码

#运行 es
nohup ./elasticsearch &
tail -f nohup.out
```

配置分词器插件

```bash
1.把之前的压缩包上传到plugin目录
2.unzip elasticsearch-analysis-ik-7.17.9
3.修改ik目录下plugin-descriptor.properties的elasticsearch.version=7.17.9
```

#### 本地使用kibana远程连接es

修改本地kibana  config/yml

```yaml
elasticsearch.hosts: ["http://114.115.168.139:9200"]
elasticsearch.username: "kibana_system"
elasticsearch.password: "W36TSKN^t*PjSZSD9JYEY#FB*KKT&J"
账号 elastic 密码 W36TSKN^t*PjSZSD9JYEY#FB*KKT&J
```

####  创建索引结构、设置分词器

``` json
PUT /question_v1
{
  "settings": {
    "analysis": {
      "analyzer": {
        "index_analyzer": {
          "type": "ik_max_word",  
          "stopwords": "_chinese_"  
        },
        "search_analyzer": {
          "type": "ik_sm"  
        }
      }
    }
  },
  "mappings": {
    "properties": {
      "title": {
        "type": "text",
        "analyzer": "index_analyzer",  
        "search_analyzer": "search_analyzer"
      },
      "content": {
        "type": "text",
        "analyzer": "index_analyzer",  
        "search_analyzer": "search_analyzer"
      },
      "tags": {
        "type": "keyword"
      }
    }
  }
}
```



###  踩坑

1. 启动报错：`org.elasticsearch.bootstrap.StartupException: java.lang.RuntimeException: can not run elasticsearch as root` es不能用root用户启动，需要重新创建用户。

```bash
adduser es #新增用户

passwd 密码

chown -R 用户名:密码 elasticsearch-7.17.9/

chmod 770 elasticsearch-7.17.9/ #更改权限

su es

重新启动
```

2. 启动报错：`bootstrap check failure [1] of [2]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]`

```bash
vim /etc/sysctl.conf

#添加参数
vm.max_map_count = 262144
```

3. 启动报错：`java.lang.IllegalStateException: failed to obtain node locks, tried [[/usr/share/elasticsearch/data]] with lock id [0]; maybe these locations are not writable or multiple nodes were started without increasing [node.max_local_storage_nodes] (was [1])?`

```bash
ps -aux | grep elasticsearch
kill -9 进程号
```

4. 安装ik分词器插件启动后报错

版本

elasticsearch : 7.17.9

elasticsearch-analysis-ik: 7.17.7

将ik分词器插件放在plugins上时elasticsearch启动失败

**原因：**

两个版本不一致导致的问题

之所以没有下载相同版本的插件是因为插件没有7.17.9这个版本

**解决方法：**

1.使用相同版本的包
2.在plugin-descriptor.properties中的elasticsearch.version=XXX修改为es的版本
