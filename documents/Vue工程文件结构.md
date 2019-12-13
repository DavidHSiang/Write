## Vue.js文件结构

-----

### 一、创建/运行vue项目

#### (1) 创建vue项目

```sh
# 创建一个基于 webpack 模板的新项目
vue init webpack my-project
```

这里需要进行一些配置，默认回车即可:

![创建vue工程](../pictures/vue.js文件结构/创建vue工程.png)

![创建vue工程完毕](../pictures/vue.js文件结构/创建vue工程完毕.png)

#### (2)运行vue项目

```sh
# 打开工程目录
cd my-project

# 启动工程
npm run dev
```

成功执行以上命令后访问 http://localhost:8080/ ，输出结果如下所示：

![vue工程启动结果](../pictures/vue.js文件结构/vue工程启动结果.png)

### 二、Vue.js 目录结构

#### (1) 用 IDE 打开工程

使用Atom(IDE)打开工程文件夹，可看到工程目录

![vue工程目录](../pictures/vue.js文件结构/vue工程目录.png)

#### (2) 目录解析

| 目录/文件    | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| build        | 项目构建(webpack)相关代码                                    |
| config       | 配置目录，包括端口号等。我们初学可以使用默认的。             |
| node_modules | npm 加载的项目依赖模块                                       |
| src          | 这里是我们要开发的目录，基本上要做的事情都在这个目录里。里面包含了几个目录及文件：<br/>**assets**: 放置一些图片，如logo等。 <br>**components**: 目录里面放了一个组件文件，可以不用。 <br/>**App.vue**: 项目入口文件，我们也可以直接将组件写这里，而不使用 components 目录。  <br/>**main.js**: 项目的核心文件。 |
| static       | 静态资源目录，如图片、字体等。                               |
| test         | 初始测试目录，可删除                                         |
| .xxxx文件    | 这些是一些配置文件，包括语法配置，git配置等。                |
| index.html   | 首页入口文件，你可以添加一些 meta 信息或统计代码啥的。       |
| package.json | 项目配置文件。                                               |
| README.md    | 项目的说明文档，markdown 格式                                |