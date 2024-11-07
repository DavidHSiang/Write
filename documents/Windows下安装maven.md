## Windows下安装maven

-----

### 下载

[官网网址](https://maven.apache.org)：https://maven.apache.org

[官网下载地址](https://maven.apache.org/download.cgi)：https://maven.apache.org/download.cgi

注：下载有binary和source的两个版本，binary是编译好的可以直接使用，source是还没编译过的源代码。

### 安装

#### 0. 检查JAVA_HOME环境变量

``echo %JAVA_HOME%``
输出：
``C:\Program Files\Java\jre1.8.0_231``

#### 1. 解压软件包

将软件包解压在**非中文无空格**目录下

#### 2. 配置环境变量

1. 新建变量M2_HOME或MAVEN_HOME，变量值为maven目录地址

![配置M2_HOME](http://lsky.davidhsiang.top/i/2024/11/04/6727a46783085.png)

注：MAVEN_HOME是maven 1的写法、M2_HOME是maven 2的写法。

  能使用M2_HOME就使用M2_HOME，MAVEN_HOME会带来一些莫名其妙的bug

2. 在变量名为Path下，新建变量值为maven的bin目录地址

![配置PATH](http://lsky.davidhsiang.top/i/2024/11/04/6727a469ce758.png)

#### 3. 验证环境是否配置成功

在cmd下，输入命令：``mvn -v``验证是否配置成功

![验证环境是否配置成功](http://lsky.davidhsiang.top/i/2024/11/04/6727a46f153bd.png)

#### 4. 安装完成

-----

### 参考文档

[apache-maven-3.6.0的安装配置](https://blog.csdn.net/weixin_43916850/article/details/87959273#commentBox)

[MAVEN_HOME、M2_HOME，maven环境变量如何设置](https://blog.csdn.net/qq_42145871/article/details/88893949)

[下载软件binary与source版本的区别](https://blog.csdn.net/heimao0307/article/details/79790081)
