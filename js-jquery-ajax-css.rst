Web开发技术
===========

Web标准由一系列规范组成，由于Web设计越来越趋于整体和结构化。此前的Web标准也逐渐变为由3大部分组成的标准集，即结构(Structure)，表现(Presentation)和行为(Behavior)。

- 结构化标准语言主要包括HTML和XHTML。

- 表现标准语言主要包括CSS。

- 行为标准语言主要包括对象模型(DOM)和ECMAScript(JavaScript)等。

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

**Document对象**

document是一个文档对象，使用document对象可以对HTML文档进行检查，修改或添加内容等操作，并且可以处理该文档内部的事件。

- 属性: title, bgColor, fgColor, linkColor, alinkColor, vlinkColor, URL, fileCreatedDate, fileModifiedDate, charset, fileSize, cookie

- 方法: 
    - write() : 动态向页面写入内容
    - createElement() : 创建一个HTML标签对象
    - getElementById(id) : 获得指定id值的对象
    - getElementByName(name) : 获得指定name值的对象

**location对象**

location对象包含当前对象的URL，该对象有一个常用的href属性和reload方法。

href属性用于跳转到指定的网页，相当于<a>标签的功能。例如，要把当前页面跳转到default.html，则实现代码如下:
::

    window.location.href = "default.html";

完整代码如下:
::

    <body>
        <a href="#" onClick="Javascript:window.location.href='default.html'">按此处到default页面</a>
    </body>

**其他对象**

- history对象
- external对象
- screen对象

DOM
-----

DOM(Document Object Model) is a cross-platform and language-independent convention for representing and interacting with objects in HTML, XHTML and XML documents. Objects in the DOM tree may be addressed and manipulated by using methods on the objects. The public interface of a DOM is specified in its application programming interface(API).

从1998年W3C发布第一级DOM规范到现在，共有3个DOM级别，它们分别是DOM Level 1, DOM Level 2和DOM Level 3。

- DOM Level 1以映射文档结构为目标，由DOM核心和DOM HTML两个模块组成。
- DOM Level 2添加了命名空间支持，增加了几个模块以支持级联样式表，事件以及增强树的操作。
- DOM Level 3通过引入统一方式载入，保存和检验文档方法对DOM进行进一步扩展，扩展后，DOM核心可支持XML1.0的所有内容，包括XML Infoset, XPath和XML Base。

另外，DOM按照标准不同被分为不同的部分，即DOM Core, XML DOM和HTML DOM等。其中，DOM Core定义了一套标准的针对任何结构化文档的对象；XML DOM定义了一套标准的针对XML文档的对象；HTML DOM定义了一套标准的针对HTML文档的对象。

HTML DOM中的节点树
^^^^^^^^^^^^^^^^^^^^

在DOM中，HTML文档的层次结构被映射为一个树形结构，文档的每一个成分都是这棵树中的节点(node)。其中，整个文档是一个文档节点(Document)，每个HTML标签是一个元素节点(Element)，每个标签中的属性是一个属性节点(Attr)，文本是一个文本结点(Text)，注释属于注释节点(Comment)。

DOM的4个基本接口
^^^^^^^^^^^^^^^^^^

DOM利用对象将文档模型化，这些模型不仅描述了文档的结构，还定义了模型中对象的行为。也就是说，在DOM中，节点不仅仅是数据结构中的节点，而是对象，对象中包含属性和方法。在DOM中，对象模型要实现用来表示和操作文档的接口，接口的行为和属性，接口之间的关系以及互操作。

在DOM接口规范中，有Document,Node,NodeList以及NamedNodeMap这4个基本接口:

- **Document接口**

Document接口代表了整个文档，它是整棵文档树的根，提供了对文档树中的节点进行访问和操作的入口。

- **Node接口**

DOM接口中有很大一部分接口是从Node接口继承过来的。例如，Element,Attr,CDATASection等接口都是从Node继承过来的。在DOM树中，Node接口代表了树中每个节点，提供了访问DOM树中各个节点的属性和方法，并给出了对DOM树中的元素进行遍历的支持。

- **NodeList接口**

NodeList接口提供了对节点集合的抽象定义，它并不包含如何实现这个节点集合的定义。NodeList用于表示有顺序关系的一组节点，如某个节点的子节点序列。另外，它还出现在一些方法的返回值中，如getElementsByTagName()。

- **NamedNodeMap接口**

通过NamedNodeMap接口，可以建立节点名和节点之间的一一映射关系，从而利用节点名可以直接访问特定的节点，这个接口主要用在属性节点的表示上。

DOM基本对象
^^^^^^^^^^^^^

DOM中的基本对象有5个，即Document, Node, NodeList, Element和Attr。

CSS
-----

CSS是Cascading Style Sheet的缩写，中文为"层叠样式表"，是一组格式设置规则，用于控制网页的显示。通过CSS可以将网页内容与表现形式分离，使网页更加简练，缩短加载时间，还能够使得站点外观的维护更加容易。

CSS的最大优势有:

- 样式重用: 编写好的样式(CSS)文档，可以被用于多个HTML文档，样式就达到了重用的目的，节省了编写代码的时间，统一了多个HTML文档的样式。

- 轻松地增加网页的特殊效果: 使用CSS标记，可以非常简单地对图片，文本信息进行修饰，设置相关属性。

- 使元素更加准确定位: 使显示的信息按设计人员的意愿出现在指定的位置。

- 功能强大: 能够控制所有出现在网页中的外观与布局，并且可以为每种元素所需要的样式。

- 易于维护和改版: 如需进行全局的更新，只需简单地改变样式，然后网站中的所有元素均会自动更新。
