## Vue.js 条件语句

-----

### 一、``v-if``指令

在html标签中使用``v-if=" "``，Vue会对" "(引号)中的内容进行计算，并通过计算结果，判断标签内容是否可见

> [实例](../examples/Vue条件语句/v-if实例.html)：v-if 实例

``` html
<div id="app">
    <p v-if="seen">hello!</p>
    <button @click="seen = !seen">点击按钮显示或隐藏"hello!"</button>
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            seen: true
        }
    })
</script>
```

在上述实例中：

通过``<button>``来控制``seen``的属性值

v-if 指令根据表达式 seen 的值(true 或 false )来决定是否插入 p 元素。

### 二、``v-else``指令

我们可在拥有``v-if``或``v-else-if``指令的html元素后，紧跟一个带有``v-else``的元素。用作 ``v-if`` 的 "else" 块

>[实例](../examples/Vue条件语句/v-else实例.html)：v-else 实例

``` html
<div id="app">
    <p v-if="seen">true</p>
    <p v-else>false</p>
    <button @click="seen = !seen">点击按钮切换显示标签"</button>
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            seen: true
        }
    })
</script>
```

### 三、``v-else-if``指令

我们可在拥有``v-if``或``v-else-if``指令的html元素后，紧跟一个带有``v-else-if``的元素。用作 ``v-if`` 的 "else-if" 块。可以链式的多次使用

> [实例](../examples/Vue条件语句/v-else-if实例.html)：v-else-if 实例

``` html
<div id="app">
    <div v-if="type === 'A'">
        A
    </div>
    <div v-else-if="type === 'B'">
        B
    </div>
    <div v-else-if="type === 'C'">
        C
    </div>
    <div v-else>
        Not A/B/C
    </div>
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            type: 'C'
        }
    })
</script>
```

> **注意：** ``v-else`` 、``v-else-if`` 必须跟在 ``v-if`` 或者 ``v-else-if`` 之后。

### 四、``v-show``指令

我们也可以使用 v-show 指令来根据条件展示元素：

Vue会对``v-show = " "``中的内容进行运算，若得出的结果为``true``则进行显示，若为``false``则不显示

> [实例](../examples/Vue条件语句/v-show实例.html)：v-show 实例

``` html
<div id="app">
    <p v-show="seen">hello!</p>
    <button @click="seen = !seen">点击按钮显示或隐藏"hello!"</button>
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            seen: true
        }
    })
</script>
```

-----

### 参考文档

[Vue.js 条件语句](https://www.runoob.com/vue2/vue-if.html)