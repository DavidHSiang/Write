## Windows下安装node.js

-----

### 1. 下载地址

* 官网下载地址：https://nodejs.org/en/download/

### 2. 安装

* 一切按默认安装

### 3. 检查安装是否成功

* 检测环境变量

查看环境变量中是否有：

1. ``C:\Users\<Username>\AppData\Roaming\npm``

2. <node.js安装目录>

* cmd下检测node版本

```
> node -v
v12.13.1
```

* cmd下检测npm版本

```
> npm -v
6.12.1
```

-----

### 4. 修改配置

配置npm在安装全局模块时的路径和缓存cache的路径

1. 在node.js安装目录下新建两个文件夹 node_global和node_cache
2. 在cmd命令下执行如下两个命令：

``npm config set prefix "<nodejs安装路径>\node_global"``

``npm config set cache "<nodejs安装路径>\node_cache"``

3. 在环境变量 -> 系统变量中新建一个变量名为 “NODE_PATH”， 值为``<nodejs安装路径>\node_modules``
4. 最后编辑用户变量里的Path，将相应npm的路径改为：``<nodejs安装路径>\node_global``，如下：

    更改前：
  ``C:\Users\<Username>\AppData\Roaming\npm``

    更改后：
  ``<nodejs安装路径>\node_global``

5. 配置完成

-----

### 参考文档

[node.js 安装详细步骤教程](https://blog.csdn.net/antma/article/details/86104068)
