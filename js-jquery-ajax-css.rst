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

JavaScript中，对象就是属性和方法的集合。属性表示的是对象的特征，作为信息的载体，从而与变量相关联。方法表示对象所具有的功能，从而与特定的函数关联。

**String对象** : 提供对字符串进行处理的属性和方法。在使用String对象时，首先要创建一个字符串变量。
::

    newstring = new String("This is a new string");

或:
::

    newstring = "This is a new string";

**Math对象** : 主要提供一些基本的数学函数和常数。

**Date对象** : 主要用于设置和获取日期的年，月，日，时，分，秒，毫秒等。

创建Date对象的常见格式有以下3种:
::
    var date = new Date();
    var date = new Date(2010,1,1);
    var date = new Date(2010,1,1,12,30,20,40);

需要注意的是，Date对象没有提供可以直接访问的属性，只具有获取和设置日期和时间的方法。

**Array对象** : Array对象最常用的属性和方法为length和toString()。length返回数组中元素的个数; toString()返回一个字符串，该字符串中包含数组中的所有元素，用逗号分隔。

建立数组对象的格式有以下3种:
::

    数组对象名称 = new Array([元素个数]);
    数组对象名称 = new Array([元素1],[元素2]...);
    数组对象名称 = [元素1[,元素2,...]];

自定义对象
^^^^^^^^^^^

在JavaScript中定义自己的对象有以下两种方法:

- 通过构造函数

使用new关键字和构造函数创建对象的实例，语法格式如下:
::

    var student = new Student();

示例代码如下:
::

    <script language="javascript">
        function Student()
        {}

        function test()
        {
            alert(this["name"] + ": " + this["sex"]);
        }

        var student = new Student();
        student.name = "xiaoming";
        student.sex = "male";
        student.T = test;
        student.T();
    </script>

- 使用Object对象

Object("O"必须大写)用于提供一种创建自定义对象的简单方式，不需要再定义构造函数，其语法格式如下:
::

    var student = new Object();

示例代码如下:
::

    <script language="javascript">
        var student = new Object();
        student.name = "zhangsan";
        student.sex = "male";
        function test(x);
        {
            alert(student[x]);
        }
        test("name");
        test("sex");
    </script>

浏览器对象
^^^^^^^^^^^^

JavaScript除了可以访问本身内置的各种对象和自定义对象外，还可以访问浏览器提供的对象。通过对这些对象的访问，可以得到当前网页和浏览器本身的一些信息，并能完成相关操作。

**window对象** : 对于window对象的使用，主要集中在窗口的打开和关闭，窗口状态的设置，定时执行程序以及各种对话框的使用等4个方面。

每一个window对象都代表一个浏览器窗口，如果要访问其内部的其他对象，window可以省略。

window对象位于最顶层，提供了处理浏览器窗口的方法和属性。

- 属性

window对象的status和location属性能完成一些有用的任务。其中，status属性用于设置浏览器底部的状态条中所显示的信息:
::

    <script language = "javascript">
        window.status = "Welcome";
    </script>

通过对location属性赋值可以使浏览器跳转到指定的URL:
::

    location = "http://www.default.com";    //可使浏览器跳转到default页面

- 方法: open,close; alert; confirm; prompt; blur,focus; scroll; setTimeout;

**Document** 对象




