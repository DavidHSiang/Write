## vue-devtools安装教程

-----

### 前言

最近在学习Vue，为了方便调试Vue的程序，于是打算安装一个Vue的调试插件：vue-devtools

在网上找了一堆使用npm安装的方法，没有一个是有用的，总是会报一些莫名其妙的错误。

看了很多vue-devtools安装教程后，我突然发现，，，**vue-devtools是一个浏览器插件**！？！？！？

亏我在这瞎鸡儿弄了这么久。直接上**Chrome 网上应用店**上下不就好了。。。

如果上不了Chrome 网上应用店的，可以使用它的国内镜像：[Chrome 网上应用店](https://www.gugeapps.net/)

或者说，直接使用**火狐浏览器**，直接在应用商店上下载就完事了

### 一、简单版安装

在**Chrome 网上应用店**或**火狐插件商店**上直接下载vue-devtools

### 二、npm安装

暂时还没找到完全不报错的方法。。找到了再更新

### 三、安装后遇到的问题

#### (1) Chrome

##### 1. 打开本地的vue网页时，插件图标为灰色

在管理扩展程序中，设置Vue插件：**允许访问文件网址**

![Chrome允许访问文件网址](../pictures/Vue-DevTools安装教程/Chrome允许访问文件网址.png)

##### 2. 打开vue网页时，插件图标为亮了，但是开发者工具中没有Vue选项

那是因为你的Vue工程没有开启调试功能，在Vue工程文件中添加``Vue.config.devtools = true``即可

```html
<body>
	<div id = "app">
        ........
    </div>
    
    <script>
    	........
        Vue.config.devtools = true
    </script>
</body>
```

#### (2) Firefox

##### 1. 打开vue网页时，插件图标为亮了，但是开发者工具中没有Vue选项

有两种可能：

1. 和Chrome一样，添加``Vue.config.devtools = true``即可
2. 第一次安装vue-devtools后，需**重启浏览器**，否则开发者工具中没有Vue选项
