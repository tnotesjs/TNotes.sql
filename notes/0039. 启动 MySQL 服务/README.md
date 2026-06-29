# [0039. 启动 MySQL 服务](https://github.com/tnotesjs/TNotes.sql/tree/main/notes/0039.%20%E5%90%AF%E5%8A%A8%20MySQL%20%E6%9C%8D%E5%8A%A1)

<!-- region:toc -->

- [1. 评价](#1-评价)

<!-- endregion:toc -->

## 1. 评价

- macOS 环境的基本操作流程：
  - 检查是否已经安装了 mysql：
    - `which mysql`
      - 打印 mysql 的安装位置
    - `mysql --version`
      - 打印 mysql 的版本信息
  - 连接 mysql 服务器：
    - `mysql -u root -p`
      - 这是一条用于连接 MySQL 数据库服务器的命令，常用于登录数据库并进行操作。
      - `mysql` MySQL 提供的客户端命令行工具，用于与 MySQL 服务器交互。
      - `-u root` 指定以用户名为 root 的身份登录（-u 表示 user）。
      - `-p` 表示需要输入密码（password）进行认证。
  - 输入正确的密码后，将进入 MySQL 的交互式命令行界面：
    - `mysql>`
    - 接下来你就可以自由输入 mysql 命令来和数据库之间进行交互了。
  - ![图 1](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-05-21-20-25-58.png)
