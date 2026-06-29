# [0044. 在 macOS 上安装 mysql](https://github.com/tnotesjs/TNotes.sql/tree/main/notes/0044.%20%E5%9C%A8%20macOS%20%E4%B8%8A%E5%AE%89%E8%A3%85%20mysql)

<!-- region:toc -->

- [1. 评价](#1-评价)
- [2. 直接到官网下载 mysql 并安装](#2-直接到官网下载-mysql-并安装)
- [3. 通过 homebrew 安装 mysql](#3-通过-homebrew-安装-mysql)

<!-- endregion:toc -->

## 1. 评价

- 介绍在 macOS 上安装 mysql 的基本流程。

## 2. 直接到官网下载 mysql 并安装

- https://dev.mysql.com/downloads/
  - ![图 0](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-05-23-08-10-10.png)
- 根据你的系统选择对应的安装包下载：
  - ![图 1](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-05-23-08-10-45.png)
- 以 macOS 为例，安装流程如下：
  - 解压下载的安装包，双击 `mysql-commercial-9.3.0-macos15-arm64.dmg` 安装 mysql。
  - 安装流程非常简单，全程点击右下角的“继续”即可，中途需要输入 PC 密码以及设置你的数据库 root 用户的密码。
  - ![图 2](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-05-23-08-11-46.png)
  - ![图 4](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-05-23-08-12-59.png)
  - ![图 3](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-05-23-08-12-51.png)
- 验证是否安装成功：

```bash
# 打印 mysql 的安装路径
$ which mysql
# /usr/local/mysql/bin/mysql

# 打印 mysql 的版本信息
$ mysql --version
# mysql  Ver 9.3.0-commercial for macos15 on arm64 (MySQL Enterprise Server - Commercial)

# 登录 mysql
$ mysql -u root -p
# Enter password:
# Welcome to the MySQL monitor.  Commands end with ; or \g.
# Your MySQL connection id is 10
# Server version: 9.3.0-commercial MySQL Enterprise Server - Commercial

# Copyright (c) 2000, 2025, Oracle and/or its affiliates.

# Oracle is a registered trademark of Oracle Corporation and/or its
# affiliates. Other names may be trademarks of their respective
# owners.

# Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

# mysql>
```

- 注册 Oracle 账号：
  - 这个步骤是可选的，并非一定要有 Oracle 账号才能下载 mysql。
  - 如果需要注册的话，可以随便填写一下表单，提交后去邮箱里面激活一下即可。
  - ![图 5](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-05-23-08-17-04.png)

## 3. 通过 homebrew 安装 mysql

- `brew install mysql`
- 如果是 `macOS 15.x.x` 那么可能会出现如下错误：
  - `Error: unknown or unsupported macOS version: :dunno`
  - 因为 `macOS 15.x.x` 是一个预览版本，，而 Homebrew 尚未正式支持这个版本。

```bash
$ brew install mysql
# Warning: You are using macOS 15.
# We do not provide support for this pre-release version.
# It is expected behaviour that some formulae will fail to build in this pre-release version.
# It is expected behaviour that Homebrew will be buggy and slow.
# Do not create any issues about this on Homebrew's GitHub repositories.
# Do not create any issues even if you think this message is unrelated.
# Any opened issues will be immediately closed without response.
# Do not ask for help from Homebrew or its maintainers on social media.
# You may ask for help in Homebrew's discussions but are unlikely to receive a response.
# Try to figure out the problem yourself and submit a fix as a pull request.
# We will review it but may or may not accept it.
#
# Error: unknown or unsupported macOS version: :dunno
```
