## Vue.js安装

-----

### 一、独立版本

我们可以在 Vue.js 的官网上直接下载 vue.min.js 并用 ``<script> ``标签引入。

官网下载：https://vuejs.org/js/vue.min.js

### 二、使用 CDN 方法

以下推荐国外比较稳定的两个 CDN，目前还是建议下载到本地。

* Staticfile CDN（国内） : https://cdn.staticfile.org/vue/2.2.2/vue.min.js
* unpkg(和npm的最新版本保持一致)：https://unpkg.com/vue/dist/vue.js
* cdnjs : https://cdnjs.cloudflare.com/ajax/libs/vue/2.1.8/vue.min.js

### 三、 NPM 方法

#### (1) 配置淘宝 npm 镜像

由于 npm 安装速度慢，在安装前，需要使用淘宝 npm 镜像，详见[NPM使用教程](NPM使用教程.md)

#### (2) 升级 npm 版本

npm 版本需要大于 3.0，如果低于此版本需要升级它：

```sh
# 查看版本
$ npm -v
2.3.0

#升级 npm
cnpm install npm -g


# 升级或安装 cnpm
npm install cnpm -g
```

#### (3) 安装vue：

```sh
# 最新稳定版
cnpm install vue

# 全局安装 vue-cli
cnpm install --global vue-cli
```

#### (4)检测安装是否成功

输入命令：``vue -V``

若显示vue版本号，则代表安装成功

**注意**：``-V``一定要是**大写**的"V"