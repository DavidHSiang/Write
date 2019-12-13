## HTML \<label> 标签的 for 属性

[HTML <label> 标签](https://www.w3school.com.cn/tags/tag_label.asp)

-----

### 定义和用法

* 定义：for 属性规定 label 与哪个表单元素绑定

#### 隐式和显式的联系

标记通常以下面两种方式中的一种来和表单控件相联系：

* 隐式形式：将表单控件作为标记标签( <label> 标签)的内容

* 显式形式： <label> 标签下的 for 属性命名一个目标表单 id

``` html
<!--显式的联系：-->
<label for="SSN">Social Security Number:</label>
<input type="text" name="SocSecNum" id="SSN" />

<!--隐式的联系：-->
<label>Date of Birth: <input type="text" name="DofB" /></label>
```

-----

### 语法

```
<label for="value">
```

#### 属性值

| 值           | 描述                      |
| ------------ | ------------------------- |
| *element_id* | label 要绑定的元素的 id。 |

-----

### 实例

```html
<html>
    <body>

        <p>请点击文本标记之一，就可以触发相关控件：</p>

        <form>
            <label for="male">
                Male
            </label>
            <input type="radio" name="sex" id="male" />
            <br />
            <label for="female">
                Female
            </label>
            <input type="radio" name="sex" id="female" />
        </form>

    </body>
</html>
```

-----

### 浏览器支持

所有主流浏览器都支持 for 属性。

Safari 2 或更早的版本不支持 for 属性。

-----

### 参考文档

[HTML <label> 标签的 for 属性](https://www.w3school.com.cn/tags/att_label_for.asp)