## VS报错-TCHAR\*的实参与char\*的形参不兼容

### char、CHAR、TCHAR、WCHAR的关系

char：
* C语言标准数据类型，字符型
* 由几个字节组成通常由编译器决定，一般一个字节。

Windows为了消除各编译器的差别，重新定义了一些数据类型

CHAR
* ANSI字符
* 单字节字符

WCHAR
* Unicode字符
* 不论中英文，每个字有两个字节组成。

TCHAR：

* 如果当前编译方式为ANSI（默认）方式，TCHAR等价于CHAR
* 如果为Unicode方式，TCHAR等价于WCHAR

LPCSTR：
* 相当于CONST CHAR \*

LPSTR：
* 相当于CHAR \*

### 错误的原因

当遇到报错：``TCHAR\*的实参与char\*的形参不兼容``时，是因为使用的函数需要的参数类型是ANSI方式，而编译器默认的是Unicode方式的。

此时编译方式出现问题，因此我们设置一下编译方式即可。

### 解决方法

采用将编译方式设置为ANSI：

``项目->属性->项目默认值->字符集，然后把UNICODE选成多字节，重新编译``

这样我们就完成了设置，并且Error得到了解决。

-----

### 参考文档

[Error:"_TCHAR\*\*"的实参与"char\*\*"的形参不兼容](https://blog.51cto.com/10115411/1631144)：https://blog.51cto.com/10115411/1631144
