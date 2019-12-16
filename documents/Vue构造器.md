## Vue构造器

-----

### 一、什么是Vue 构造器

每个Vue.js 应用都是通过构造函数Vue()，实例化Vue来实现的。
<!--这里对DOM的描述是否正确有待研究-->
通过Vue实例，将Vue实例的成员与DOM相关联

语法格式如下：

```javascript
var vm = new Vue({
  // 参数
})
```

### 二、Vue构造器的参数

在介绍Vue构造器的参数之前，先看一看Vue的测试实例。通过实例，来逐步介绍Vue构造器的参数：

```vue
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Vue 测试实例</title>
    <script src="https://cdn.staticfile.org/vue/2.4.2/vue.min.js"></script>
</head>
<body>
    <div id="vue_det">
        <h1>hello : {{hello}}</h1>
        <h1>helloWorld(): {{helloWorld()}}</h1>
        <h1>reversedMessage(): {{reversedMessage}}</h1>
        <h1>hello | capitalize: {{hello | capitalize}}</h1>
        千米 : <input type = "text" v-model = "kilometers">
        米 : <input type = "text" v-model = "meters">
	</div>
    <p id = "info"></p>
	<script type="text/javascript">
		var vm = new Vue({
			el: '#vue_det',
			data: {
                hello: "hello",
                kilometers : 0,
                meters:0
			},
			methods: {
				helloWorld: function() {
					return  this.hello + " World!";
				}
			},
            computed: {
                reversedMessage:function(){
                    return this.hello.split('').reverse().join('')
                }
            },
            filters: {
                capitalize: function (value) {
                    if (!value) return ''
                    value = value.toString()
                    return value.charAt(0).toUpperCase() + value.slice(1)
                }
            },
            watch : {
                kilometers:function(val) {
                    this.kilometers = val;
                    this.meters = this.kilometers * 1000
                },
                meters : function (val) {
                    this.kilometers = val/ 1000;
                    this.meters = val;
                }
            }
		})
        // $watch 是一个实例方法
        vm.$watch('kilometers', function (newValue, oldValue) {
            // 这个回调将在 vm.kilometers 改变后调用
            document.getElementById ("info").innerHTML = "修改前值为: " + oldValue + "，修改后值为: " + newValue;
        })
	</script>
</body>
</html>
```

这个实例结合了我目前学过的所有Vue构造函数的参数，下面对他们一 一进行介绍：

#### (1) ``el``参数

* ``el``参数：它是 DOM 元素中的 id，在上面实例中 id 为 ``vue_det``，在 div 元素中：

  ```html
  <div id = "vue_det"></div>
  ```

  这意味**将Vue实例``vm``和div 元素相关联**，``vm``中的改动只影响在以上指定的 div 内，div 外部不受影响

#### (2) ``data``参数

* ``data``参数：用于定义vm的属性，``data``中所定义的所有内容都将作为vm对象的属性

#### (3) ``methods``参数

* ``methods``参数：用于定义vm的方法，``methods``中所定义的所有内容都将作为vm对象的方法

#### (4) ``computed``参数

* ``computed``参数：定义vm的计算属性，在``computed``中定义的方法将作为vm对象的属性

  在上面实例中，``computed``属性定义的内容为：

  ``` javascript
  computed: {
                  reversedMessage:function(){
                      return this.hello.split('').reverse().join('')
                  }
              }
  ```

  它的作用是，为vm对象创建一个名为reversedMessage属性。将提供的函数作为``vm.reversedMessage``的``getter``。

  ``computed``属性默认只有``getter``，不过在需要时你也可以提供一个``setter``：

  ``` javascript
  computed: {
      reversedMessage: {
          // getter
          get: function () {
              return this.reversedMessage
          },
          // setter
          set: function (newValue) {
              this.reversedMessage = newValue
          }
      }
  }
  ```

##### (4.1) ``methods``参数 与 ``computed``参数 的区别

​	我们可以使用 methods 来替代 computed，效果上两个都是一样的，但是 computed 是基于它的依赖缓存，只有相关依赖发生改变时才会重新取值。而使用 methods ，在重新渲染的时候，函数总会重新调用执行。

#### (5) ``filters``参数

* ``filters``参数：定义vm的过滤器

  在上面实例中，``filters``属性定义的内容为：

  ``` javascript
  filters: {
                  capitalize: function (value) {
                      if (!value) return ''
                      value = value.toString()
                      return value.charAt(0).toUpperCase() + value.slice(1)
                  }
              }
  ```

  它的作用是，为vm对象创建一个名为``capitalize``的过滤器。``capitalize``的过滤器的功能为：对输入的字符串第一个字母转为大写。

  通过**"管道符"(``|``)**可调用过滤器。如上面实例中，通过：

  ```html
  <h1>hello | capitalize: {{hello | capitalize}}</h1>
  ```

  格式化输出 hello 属性的值，输出为：``hello | capitalize: Hello``

  过滤器可以串联：

  ```javascript
  {{ message | filterA | filterB }}
  ```

  过滤器是 JavaScript 函数，因此可以接受参数：

  ```
  {{ message | filterA('arg1', arg2) }}
  ```

  这里，message 是第一个参数，字符串 'arg1' 将传给过滤器作为第二个参数， arg2 表达式的值将被求值然后传给过滤器作为第三个参数。

#### (6) ``watch``参数

* ``watch``参数：``watch``用于对vm的属性建立监听

  如上面的实例中：

  在``data``中定义了两个vm属性：
  
  ```javascript
  data: {
                  kilometers : 0,
                  meters:0
  			}
  ```
  
  在watch中定义了两个监听方法：
  
  ```javascript
  watch : {
                  kilometers:function(val) {
                      this.kilometers = val;
                      this.meters = this.kilometers * 1000
                  },
                  meters : function (val) {
                      this.kilometers = val/ 1000;
                      this.meters = val;
                  }
              }
  ```
  
  它实现了对``vm.kilometers``和``vm.meters``进行监听，当属性值被外界修改时，调用对应的方法

#### (x) 附加说明

##### 1. {{ }}

``{{ }}``用于输出对象属性和函数返回值

##### 2.``vm.$watch()``方法

vue实例可以通过``m.$watch()``方法来对属性进行监听

如上述实例中：

``` javascript
// $watch 是一个实例方法
vm.$watch('kilometers', function (newValue, oldValue) {
	// 这个回调将在 vm.kilometers 改变后调用
    document.getElementById ("info").innerHTML = "修改前值为: " + oldValue + "，修改后值为: " + newValue;
})
```

它的作用是，对vm的``kilometers``属性进行监听，当``kilometers``改变时，调用这里定义的方法，``newValue``参数是``kilometers``属性改变后的值，``oldValue``参数是``kilometers``属性改变前的值

##### 3. ``split()``方法

``split()`` 方法用于把一个字符串分割成字符串数组。

##### 4. ``reverse()``方法

``reverse()`` 方法用于颠倒数组中元素的顺序。

##### 5. ``join()`` 方法

``join()`` 方法用于把数组中的所有元素放入一个字符串。

-----

### 参考文档

[菜鸟教程-Vue.js教程](https://www.runoob.com/vue2/vue-tutorial.html)

​	[Vue.js 起步](https://www.runoob.com/vue2/vue-start.html)

​	[Vue.js 模板语法](https://www.runoob.com/vue2/vue-template-syntax.html)

​	[Vue.js 计算属性](https://www.runoob.com/vue2/vue-computed.html)

​	[Vue.js 监听属性](https://www.runoob.com/vue2/vue-watch.html)

​	[Vue.js 事件处理器](https://www.runoob.com/vue2/vue-events.html)

[Vue 构造器](https://blog.csdn.net/yana_loo/article/details/76997094)

[split().reverse().join()放一起 好记很多](https://blog.csdn.net/qq_35209064/article/details/85795376)