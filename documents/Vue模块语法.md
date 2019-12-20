## Vue模块语法

-----

[TOC]

-----

### 一、插值

#### (1) ``{{ }}`` 文本插值

``{{ }}``用于输出对象属性和函数返回值。``{{ }}``中的内容可以变量、值或表达式，Vue会对``{{ }}``中的内容进行计算，将计算的结果以**文本**的形式插入到网页中

> [实例](../examples/Vue模块语法/文本插值实例.html)：文本插值

``` vue
<div id = "app" style="margin-left: 300px;">
    <h2>
        文本插值
    </h2>

    <p>变量：{{num}}</p>
    <p>表达式：{{ 5 + 10 }}</p>
    <p>三目运算符：{{ true? 15 : 10}}</p>
    <p>函数：{{ getNum() }}</p>
    <p>匿名函数：{{ (() => 5 + 10)() }}</p>
    <p>对象：{{ {num : 15} }}</p>
    <p>函数对象：{{ getNum }}</p>
    <p>html代码(表达式)：{{ '<span> 15 </span>' }}</p>
    <p>html代码(变量)：{{ html }}</p>
</div>

<script type="text/javascript">
    var vm = new Vue({
        el:"#app",
        data:{
            num : 15,
            html:'<span> 15 </span>'
        },
        methods: {
            getNum : function(){
                return this.num
            }
        }
    })
</script>
```

#### (2) ``v-html`` 输出 html 代码

``v-html``用于在网页中插入html语句。在html中使用``<div v-html=" "></div>``语句，Vue会对``" "``(引号)中的内容进行计算，将计算的结果以**html**的形式插入到网页中

> [实例](../examples/Vue模块语法/html插值实例-单行html.html)：插入单行html

``` html
<div id="app">
            <div v-html="message"></div>
        </div>

<script>
    new Vue({
        el: '#app',
        data: {
            message: '<h1>Hello World!</h1>'
        }
    })
</script>
```

当要插入多行html语句时，可以使用**\`多行内容\`**来实现

> [实例](../examples/Vue模块语法/html插值实例-多行html.html)：插入多行html

``` html
<div id="app">
    <div v-html="message"></div>
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            message: `
                    <h1>标题h1</h1>
                    <h2>标题h2</h2>
                    <h3>标题h3</h3>
                    <h4>标题h4</h4>
                    `
        }
    })
</script>
```

#### (3) ``v-bind`` 绑定属性

``v-bind``用于单向绑定html标签的属性。在html标签中使用``v-bind:属性名=" "``语句，Vue会对``" "``(引号)中的内容进行计算，将计算的结果作为该属性的**属性值**

> [实例](../examples/Vue模块语法/v-bind单向绑定属性.html)：``v-bind`` 绑定属性

``` html
<div id="app">
    <p>{{ message }}</p>
    <input v-bind:value="message">
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            message: 'Hello!'
        }
    })
</script>
```

运行实例可见：输入框的内容修改时，第一个文本的内容不会修改。这是因为message属性的值并没有被改变。因此``v-bind`` 绑定是单向的

### 二、用户输入

#### (1) ``v-model`` 双向数据绑定

##### (1.1) ``v-model`` 基础

上面讲了使用``v-bind`` 进行单向绑定。下面来讲一讲，使用``v-model`` 进行双向数据绑定

在**input 输入框**中我们可以使用 ``v-model`` 指令来实现双向数据绑定：

> [实例](../examples/Vue模块语法/v-model 双向数据绑定.html)：``v-model`` 双向数据绑定

``` html
<div id="app">
    <p>{{ message }}</p>
    <input v-model="message">
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            message: 'Hello!'
        }
    })
</script>
```

在使用``v-model``时，我们并没有对要绑定的属性进行声明，那么我们要如何才能与标签的其他属性进行双向数据绑定呢？

这要从``v-model``的原理说起：

##### (1.2) ``v-model``原理

虽然Vue可以通过``v-model``来实现双向数据绑定，但是**Vue 是单项数据流**，``v-model`` 只是**语法糖**而已：

``` html
<input v-model="message" />
<input v-bind:value="message" v-on:input="message = $event.target.value" />
```

第一行的代码其实只是第二行的语法糖。然后第二行代码还能简写成这样：

``` html
<input :value="message" @input="message = $event.target.value" />
```

在这个语句中：

* ``:value="message"``表示将input标签的``value``属性和 Vue对象的``message``属性进行单向的绑定

  可理解为：``value = message``

* ``@input="message = $event.target.value"``表示监听input标签的``input``事件，当输入框内容发生变化时，将input标签的``value``属性赋值给Vue对象的``message``属性

  可理解为：``message = value``

**在给元素添加 v-model 属性时，默认会把 value 作为元素的属性，然后把 'input' 事件作为实时传递 value 的触发事件**

> [实例](../examples/Vue模块语法/v-model 原理.html)：``v-model`` 原理

``` html
<div id ="app">
    <input v-model="message" />
	<input v-bind:value="message" v-on:input="message = $event.target.value" />
    <!--运行实例可以看出,这两个input的效果是相同的-->
</div>

<script>
    new Vue({
                el: '#app',
                data:{
                    message:'123'
                }
            })
</script>
```

##### (1.3) ``v-model`` 缺点的解决

下面来讲解标签的其他属性进行双向数据绑定的方法，解决方法将从两个方面来讲解。一是：**v-model 用在 input 元素上时**，二是：**v-model 用在组件上时**

###### 1.v-model 用在 input 元素上时

当``v-model`` 用在 input 元素上时，由于``v-model``只是一个语法糖，也就是说：

``<input v-model="message" />`` = ``<input v-bind:value="message" v-on:input="message = $event.target.value" />``

因此我们要**放弃使用**``v-model``**转而使用**``<input :value="···" @input="···" />``**就行了**

> [例子](../examples/Vue模块语法/checkbox双向数据绑定.html)：checkbox双向数据绑定

在老版本的Vue中，对于复选框或单选框等组件不支持``v-model``

``` html
<input type="checkbox" v-model="message" />
```

这是因为：``v-model`` 给我们提供好了 ``value`` 属性和 ``oninput`` 事件，但是，**我们需要的不是 ``value`` 属性，而是 ``checked`` 属性，并且当你点击这个单选框的时候不会触发 ``oninput`` 事件，它只会触发 ``onchange`` 事件**。

因为 ``v-model`` 只是用到了 input 元素上，所以这种情况好解决：

```html
<input type="checkbox" :checked="message" @change="message = $event.target.checked" />
```

###### 2. v-model 用在组件上时

v-model 用在组件上时，v-model 默认会将Vue的属性与组件的value属性进行绑定，即：``:value``，并监听input事件，当input发生时，将input事件的附带数据复制给Vue的属性。即：``@input="message = arguments[0]"``

> 例如：

``` html
<currency-input v-model="message"></currentcy-input>
<!--上行代码是下行的语法糖-->
<currency-input :value="message" @input="message = arguments[0]"></currency-input>
```

v-model 用在组件上时，有以下三种解决方法：

1. 将**要绑定的属性设置为value**，在要监听的事件的触发方法中，**触发input事件**

> [实例](../examples/Vue模块语法/Vue 自定义组件数据绑定.html)：Vue 自定义组件数据绑定

```javascript
 Vue.component('child', {
                // 声明 value
                props:['value'],
                // 同样也可以在 vm 实例中像 “this.message” 这样使用
                methods:{
                    handleReset: function(){
                        console.log('重置为\'\'')
                        //触发input事件,并传入数据""
                        this.$emit('input','')
                    }
                },
                template: `
                <div>
                    <h2>输入值为：{{value}}</h2>
                    <button v-on:click="handleReset">重置为空</button>
                </div>
               `
            })
```

2. 同v-model 用在 input 元素上时的解决方法，**不使用v-model，而是直接使用**``v-bind``、``v-on``

> [实例](../examples/Vue模块语法/自定义组件双向绑定-不使用v-model.html)：自定义组件双向绑定-不使用v-model

```html
<div id="app">
    <child v-on:myinput = "setInput" v-bind:value="inputText" ></child>
    <input type="text" v-model="inputText" ></div>
</div>

<script>
    // 注册
    Vue.component('child', {
        // 声明 props
        props:['value'],
        // 同样也可以在 vm 实例中像 “this.message” 这样使用
        methods:{
            handleReset: function(){
                console.log('重置为\'\'')
                this.$emit('myinput','')
            }
        },
        template: `
                <div>
                    <h2>输入值为：{{value}}</h2>
                    <button v-on:click="handleReset">重置为空</button>
                </div>
               `
    })
    // 创建根实例
    new Vue({
        el: '#app',
        data:{
            inputText:''
        },
        methods:{
            setInput:function(data){
                this.inputText = data
            }
        }
    })
    Vue.config.devtools = true
</script>
```

3. 在 Vue 2.2 版本，你可以在定义组件时通过 **model 选项**的方式来定制 prop/event：

``` javascript
<my-checkbox v-model="foo"></my-checkbox>

Vue.component('my-checkbox', {
  tempalte: `<input
               type="checkbox"
               <!--这里就不用 input 了，而是 balabala-->
               @change="$emit('balabala', $event.target.checked)"
               :checked="checked"
             />`,
  props: ['checked'], //这里就不用 value 了，而是 checked
  model: {
    prop: 'checked',
    event: 'balabala'
  },
})
```

#### (2) `` v-on`` 监听事件

  ``v-on`` 用于监听事件，并对用户的输入进行响应。在html标签中使用``v-on:事件名="函数/表达式"``来对事件进行监听，当事件触发时，Vue会运行" "中的函数/表达式，来对事件进行响应。" "中的函数在Vue构造器中的``methods``参数中声明

> [实例](../examples/Vue模块语法/v-on 监听事件.html)：v-on 监听事件

``` html
<div id="app">
    <p>{{ message }}</p>
    <button v-on:click="reverseMessage">反转字符串</button>
</div>

<script>
new Vue({
  el: '#app',
  data: {
    message: 'HelloWorld!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
</script>
```

### 三、过滤器

在Vue中可以通过**过滤器**来对文本进行格式化

#### (1) 过滤器的调用

##### 1. 一般格式

过滤器的基本调用方法：

``` html
<!-- 在两个大括号中 -->
{{ message | filterA }}

<!-- 在 v-bind 指令中 -->
<div v-bind:id="message | filterA"></div>
```

在这个例子中，filterA 被定义为接收单个参数的过滤器函数，表达式 message 的值将作为参数传入到 filterA函数中。

相当于：``{{ filterA(message) }}``

##### 2. 串联

过滤器可以串联：

``` html
{{ message | filterA | filterB }}
```

在这个例子中，filterA 被定义为接收单个参数的过滤器函数，表达式 message 的值将作为参数传入到 filterA函数中。然后继续调用同样被定义为接收单个参数的过滤器函数 filterB，将 filterA 的结果传递到 filterB 中。

相当于：``{{ filterB(filterA(message)) }}``

##### 3. 多个参数

过滤器是 JavaScript 函数，因此可以接受参数：

```html
{{ message | filterA('arg1', arg2) }}
```

这里，message 是第一个参数，字符串 'arg1' 将传给过滤器作为第二个参数， arg2 表达式的值将被求值然后传给过滤器作为第三个参数。

相当于：``{{ filterA(message,'arg1', arg2) }}``

**注意：** 虽然``message | filterA``相当于``filterA(message)``，但是并不能通过``filterA(message)``的方式来调用过滤器，串联、多个参数也是同理。

#### (2) 定义过滤器

在Vue构造器中，通过``filters``参数来定义过滤器。

定义过滤器的语法格式为：

``` javascript
filters: {
    <过滤器名>: function (value) {
        <表达式>
      return <表达式>
    }
```

> [实例](../examples/Vue模块语法/过滤器-实现字符串第一个字符大写.html)：实现字符串第一个字符大写

``` html
<div id="app">
    {{ message | capitalize }}
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            message: 'hello'
        },
        filters: {
            capitalize: function (value) {
                if (!value) return ''
                value = value.toString()
                return value.charAt(0).toUpperCase() + value.slice(1)
            }
        }
    })
</script>
```

### 四、缩写

#### (1) ``v-bind`` 缩写

```html
<!-- 完整语法 -->
<a v-bind:href="url"></a>
<!-- 缩写 -->
<a :href="url"></a>
```

#### (2) ``v-on`` 缩写

``` html
<!-- 完整语法 -->
<a v-on:click="doSomething"></a>
<!-- 缩写 -->
<a @click="doSomething"></a>
```

### 五、附加说明

#### (1) 语法糖

语法糖，简单来说就是：

``代码A = 代码B + 代码C``

那么 A 就是 B+C 的语法糖

#### (2) 组件

组件是Vue的一个功能，Vue通过组件实现对html元素的扩展。

通过``Vue.component(tagName, options)``声明一个组件

通过``<tagName></tagName>``来使用组件

具体使用方法可看我的另外一篇博客：[Vue.js 组件]()

-----

### 参考文档

[Vue 进阶教程之：详解 v-model](https://www.cnblogs.com/fangshidaima/p/10066470.html)

[Vue.js 组件](https://www.runoob.com/vue2/vue-component.html)

[Vue.js 模板语法](https://www.runoob.com/vue2/vue-template-syntax.html)

[vue 过滤器基本使用](https://www.cnblogs.com/cckui/p/10342123.html)
