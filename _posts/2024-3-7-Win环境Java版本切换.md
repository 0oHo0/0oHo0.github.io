---
layout:     post
title:      Win环境Java版本切换
subtitle:   jenv的使用
date:       2024-3-7
author:     Duu
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - 后端
---

### Java多版本切换工具

Java多版本切换时，需要每次手动的修改系统环境，这很麻烦， `Jenv`是一款Java多版本切换工具

下载Releases的最新版本

[Jenv - Github](https://github.com/FelixSelter/JEnv-for-Windows)

### Jenv使用方法

相关命令：

```bash
jenv -help
JEnv is changing your environment variables. This process could take longer but it happens only when a jenv executable is found in your path
"jenv list"                            List all registered Java-Envs.
"jenv add <name> <path>"               Adds a new Java-Version to JEnv which can be refferenced by the given name
"jenv remove <name>"                   Removes the specified Java-Version from JEnv
"jenv change <name>"                   Applys the given Java-Version globaly for all restarted shells and this one
"jenv use <name>"                      Applys the given Java-Version locally for the current shell
"jenv local <name>"                    Will use the given Java-Version whenever in this folder. Will set the Java-version for all subfolders as well
"jenv link <executable>"               Creates shortcuts for executables inside JAVA_HOME. For example "javac"
"jenv uninstall <name>"                Deletes JEnv and restores the specified java version to the system. You may keep your config file
"jenv autoscan [--yes|-y] ?<path>?"    Will scan the given path for java installations and ask to add them to JEnv. Path is optional and "--yes|-y" accepts defaults.
```

### 踩坑

1. 使用`jenv`更改 JDK 版本后，`java`版本会更改，但是**不会改变**`javac`版本，会出现`Java` 版本为11，而`javac`版本仍为1.8，当手动`javac`对`.java`文件编译为`.class`文件后，使用`java`命令运行`.class`文件会报错。

> 这时手动修改`JAVA_HOME`环境变量是没有用的，需要将PATH环境变量中`javac`文件路径提前到第一个。
