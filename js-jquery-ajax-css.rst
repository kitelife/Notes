JavaScript
-----------

语法规则
^^^^^^^^^

当定义自己使用的变量，对象或函数时，名称可以由任意大小写字母，数字，下划线(_)，美元符号($)组成，但不能以数字开头，不能是JavaScript中的关键字。

JavaScript是严格区分大小写的。

变量的作用域
^^^^^^^^^^^^^

在JavaScript中，变量有全局变量和局部变量之分。全局变量定义在所有函数体之外，作用范围是其后的整个脚本部分；而局部变量则是定义在函数体之内，只对该函数有效。
::

    <script type="text/javascript">
        var JS = "javascript";
        function test() {
            var Welcome = "Welcome to JavaScript World";
            document.writeln("<li>" + JS);
            document.writeln("<li>" + Welcome);
        }
        document.writeln("<li>" + JS);
        document.writeln("<li>函数体外部无法调用局部变量");
    </script>

数据类型
^^^^^^^^^

JavaScript中允许使用4种基本的数据类型，即数值型(整数和浮点数)，字符串型(用""或''括起来的字符或数值)，布尔型(true或false)和空值。此外还有两种复合数据类型，即对象和数组，表示基础数据类型的集合。

**空值** : 就是"null"。如果一个变量的值为null，就表示它的值不是有效的对象，数字，字符串和布尔值。需要注意，空值不等同于空字符串或0。null可用于初始化变量，避免产生错误；也可以用来清空变量的内容。当把null赋值给某个变量后，这个变量就不再保存任何有效的数据了。

在JavaScript中还有一个特殊的未定义值，就是undefined，有以下情况的时候返回undefined值。

- 使用一个并未声明的变量。
- 使用一个已经声明但没有复制的变量。
- 使用的对象不存在。

运算符
^^^^^^^

JavaScript语言是弱类型语言，在不同类型变量之间进行运算时，会优先考虑字符串类型。例如，表达式8+"8"的执行结果为88。

流程控制
^^^^^^^^^^

**for in 循环语句** : for in语句是主要用来罗列对象属性的循环方式。语法格式如下:
::

    for (var 变量 in 对象)
    {
        在此执行代码;
    }

需要注意3点，第一，for in循环中所检查的对象属性并不是按照可预测的顺序来进行的；第二，它只能列举用户自定义对象的属性，包括任意继承属性，但内置对象的一些属性以及它的方法就不会被列举；第三，for in循环不能列举出未定义，也就是没有默认值的文本域
::

    var myArray = new Array();
    myArray[0] = "for";
    myArray[1] = "for in";
    myArray[2] = "jQuery";
    for(var a in myArray)
    {
        document.writeln(myArray[a] + "<br />");
    }

**with语句** : 当有一个对象的多个属性或者方法需要操作时，就可以使用with语句。

with语句其实就是为一个或一组语句指定默认对象，语法格式如下:
::

    with(object)
    {
        语句块;
    }

with语句通常用于缩短代码量，示例代码如下:
::

    <script type="text/javascript">
        var obj = document.createElement("div");
        with(obj)
        {
            style.cursor = "pointer";
            style.zIndex = "150";
            innerHTML = "abcd";
        }
        document.body.appendChild(obj);
    </script>

上述代码相当于下列代码:
::

    <script type="text/javascript">
        var obj = document.createElement("div");
        obj.style.cursor = "pointer";
        obj.style.zIndex = "150";
        obj.innerHTML = "abcd";
        document.body.appendChild(obj);
    </script>

自定义函数
^^^^^^^^^^^

函数定义通常放在<head></head>标记之间，语法格式:
::

    function 函数名()
    {
        函数体;
    }

如需使用事件调用，其语法格式如下:
::

    事件名="函数名()";

示例代码如下:
::

    <body>
        <input type="button" value="点击我" onclick="test()" />
    </body>

系统函数
^^^^^^^^^^

**eval()** : 用于计算并返回字符串表达式的值。

**parseInt()** : 用于将字符串开头的整数部分分解出来，例如:
::

    parseInt("21");
    parseInt("21.234");
    parseInt("21.234dra");

上述代码返回的都是21,而以下代码则返回NaN。
::

    parseInt("dra21");

**parseFloat()** : 用于将字符串开头的整数或浮点数都分解出来。

**escape()** : escape()函数用于将字符串中不是字母或数字的字符转换成按照格式"%XX"表示的数字，示例代码如下:
::

    var x = "Welcome To JavaScript $ World";
    alert(escape(x));

执行结果如下:
::

    Welcome%20To%20JavaScript%20%24%20World;

**unescape()** : 用于将字符串格式为"%XX"的数字转换为字符。

**isNaN()** : 用于检查一个变量是否为数值，如果是，则返回false,否则返回true.

内置对象
^^^^^^^^^^


