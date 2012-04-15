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

CSS样式规则
^^^^^^^^^^^

CSS提供了3种方式来给页面或页面元素应用样式，分别是类，标签和高级。

- 为了对网页样式定义得更为精确，有必要让相同的选择符(一般情况下为页面标记)也能分类，并能按照不同的类别来进行不同的样式设计。类就是这样一种新的样式表示符，适用于修饰页面中特定的区域。

- 标签是将页面文件中的XHTML标记重定义。用标签定义的CSS样式设置完成后，该CSS样式马上生效。例如，定义段落标记<p>样式，则可以使用标签类型。

- 高级是指将CSS样式用于特定的XHTML标记组合或ID名，如伪类选择符，ID选择符等。

无论最终选择哪一种方式，定义CSS样式规则都是相同的。每个CSS规则是一条单独的语句，用于定义如何设计样式以及应该如何应用这些样式。

CSS规则由3个部分构成，即选择符(selector)，属性(property)和值(value)，语法结构如下:
::

	选择符 {属性 ： 值}

各部分的含义如下:

- 选择符: 是指该CSS样式所要针对的对象，可以采用多种形式。

- 属性: 选择符指定标记所包含的属性，是CSS样式控制的核心，包括颜色，大小，定位，浮动和位置等。

- 值: 指定属性对应的值。如果定义了选择符的多个属性，则属性和属性值为一组，组与组之间用分号(;)隔开。

引入样式表
^^^^^^^^^^

**内联样式表**

也称为内嵌样式表，这种方式是指在HTML标记中通过style属性来设置样式表。示例如下:
::

	<h3 style="color: #fff; font-size: 11px; height: 25px; line-height: 23px;">系统设置</h3>

*内联样式只能针对指定的HTML标记定义，而不能使用CSS的类选择符或ID选择符来定义样式。*

**内部样式表**

内部样式表将所有的CSS样式都统一放到HTML页面的一个固定位置。内部样式表将作为页面的一个单独部分，通常放在head标记内，以"<style type="text/css">"标记开始，以"</style>"标记结束。

*内部样式表是CSS样式的常规应用形式，它只针对当前页面有效，因此达不到CSS样式重用的目的。*

**外部样式表**

链接外部样式表是CSS样式表应用中最好的一种形式。将CSS样式表代码单独编写在一个独立文件中，由网页进行调用，多个网页可以调用同一个外部样式表文件。因此，能够实现代码的最大化重用以及网站文件的最优化配置。

链接外部样式表具体是指在外部定义CSS样式表并形成以.css为扩展名的文件，然后在页面中通过<link>链接标记链接到页面中，而且该链接语句必须放在页面的head标记内。其语法格式如下:
::

	<link rel="stylesheet" type="text/css" href="style.css" />

选择器
^^^^^^

**全局选择器**

一旦使用全局选择器，它所定义的样式将会应用到整个页面中所有类型的单一对象上。全局选择器就是一个"*"号。

*在实际项目中，全局选择器经常会用到，通常用来做一些页面的统一和初始化工作。*

**标记选择器**

标记选择器又称为类型选择器，就是指使用标记名称作为选择符。

**类选择器**

在HTML页面中通过class属性能够实现把同样的元素进行归类，而且每个class的名称可以在页面中多次重复出现。类选择器就是用来定义这些同类元素的显示样式。

定义类选择器时，需要在自定义类的名称前面加一个句点(.)。

*类的名称可以是任意英文字符串或以英文开头与数字的组合，一般情况下是其功能及效果的简要缩写。*

**ID选择器**

在页面中，元素的ID属性指定了某个唯一元素的标识，同样可以使用该ID作为选择器来对某个特定元素定义独特的样式。

与类选择符不同，使用ID选择符定义样式时，需在ID名称前加上一个"#"号。

类选择器与ID选择器主要有以下两种区别:

- 类选择器可以给任意数量的标记定义样式，但ID选择器在页面的标记中只能使用一次。

- ID选择器对指定标记应用何种样式比类选择器具有更高的优先级。

**分组选择器**

严格地讲，分组选择器不是一种选择器，它是一种选择器的使用方法。当多个对象元素定义了相同的样式后，此时就可以使用分组选择器的方式，把它分成一组。这样做的目的，第一是为了减少不必要的字节，第二是便于简化代码。

**包含选择器**

包含选择器主要用于设置父元素下的子元素样式。包含选择器实现了跨级包含，即父对象可以包含子对象，也可以包含孙对象等。

**相邻选择器**

顾名思义，相邻选择器就是指定元素相邻的下一个元素，用"+"来表示，语法格式如下:
::

	E1+E2 {}

执行后，将选择紧跟在对象E1之后的E2对象，但是E1与E2需在同一层次结构中。

**子选择器**

子选择器也称子对象选择器，顾名思义，子对象选择器就是应用于父对象下的子元素。用">"来表示，语法格式如下:
::

	E1>E2 {}

执行后，将选择所有作为E1子对象的E2，不包括孙辈和更深的关系。

**属性选择器**

每个HTML标记都有它们各自的属性，而每个属性都具有不同的值。

属性选择器有以下4种形式:

- [att]: 匹配所有拥有att属性的元素，与属性值无关

- [att=val]: 匹配所有拥有att属性且属性值为val的元素

- [att=~val]: 匹配所有拥有att属性的元素，且val是其属性值由空格分隔开的一个完整单词。在这种情况下，val中间没有空格。

- [att|=val]: 匹配所有拥有att属性的元素，并且att的属性值是由连字符组成的，val处于连字符的开始。它主要用于通过lang属性指定特定语言的情况，如"en", "en-us", "en-gb"等。

在同一个选择器中，多个属性选择符可以同时使用。这样就可以用多个不同的属性来区分相同的元素了。下面的例子将会匹配属性class值为quote且有cite属性(不管值多少)的blockquote元素。
::

	blockquote[class=quote][cite] { color: #f00; }

**伪类选择器**

伪类选择器可以看作是一种特殊的类选择器，是能被支持CSS的浏览器自动识别的特殊选择器。伪类选择器的最大作用就是可以对链接的不同状态定义不同的样式效果。

伪类选择器定义的样式最常应用在超链接标记(<a>)上，即链接的伪类选择器。它表示链接的4种不同的状态，即未访问的链接(link)，已访问的链接(visited)，激活链接(active)和光标停留在链接上(hover)，代码如下:
::

	a:link {color:#FF0000; text-decoration:none}
	a:visited {color:#00FF00; text-decoration:none}
	a:hover {color:#0000FF; text-decoration:underline}
	a:active {color:#FF00FF; text-decoration:underline}

伪类选择器还可以和类选择器组合使用，可以在同一页面中完成几组不同的链接效果，代码如下:
::

	a.style1:link {color:red}
	a.style1.visited {color:blue}
	a.style2:link {color:yellow}
	a.style2:visited {color:green}

除了a的伪类选择器之外，CSS还支持以下伪类选择器:

- :focus : 设置对象在成为输入焦点(该对象的onfocus事件发生)时的样式表属性

- :first-child : 设置对象(Selector1)的第一个子对象(Selector2)的样式表属性

- ...

框模型
^^^^^^^

CSS的作用已经不仅仅是控制页面上元素的显示外观，更多是通过CSS来控制整个页面的布局，实现内容信息(HTML)与表现形式(CSS)的分离。

理解框模型的概念是掌握DIV+CSS制作页面的基础，它控制着页面中元素的排列和显示方式。

通常在使用CSS设计页面布局时，所有的页面元素都包含在一个矩形框内，称为元素框。元素框描述了元素及其在页面布局中所占的空间大小，因此元素框可以影响其他元素的位置及大小。

在设计页面布局时，要充分考虑所有页面元素的边框与元素框之间边距的布置才能使页面紧凑，而又不失条理。要完全理解框模型，就必须明确各边间距及填充的位置，而边距，边框，填充及内容共同构成了一个框。

.. image:: https://lh3.googleusercontent.com/-LWQN2e606Ok/T4WQqNVpM7I/AAAAAAAAA9k/WJReotF1Yc4/s433/boxdim.png

*为了更容易理解，以下内容中统一将填充称为内边距，边距称为外边距，即边框内的空白是内边距，边框外的空白是外边距。*

其实，内边距和外边距最大的区别就是**背景的显示**。

CSS中的width和height属性指的是内容区域的宽度和高度。增加边框，内边距和外边距不会影响内容区域的大小，但是会增加框的总大小。

**内边距**

内边距实际上是元素内容到边框之间的距离。通过CSS中的padding属性可以非常快捷地设置这个值。padding属性值可以是一个具体的长度，也可以是一个相对于上级元素的百分比。
::

	h3 {padding: 10px}

也可以在padding属性中按照上，右，下，左的顺序(顺时针方向)依次设置各边的内边距，而且各边均可以使用不同的单位或百分比值。
::

	h3 {padding: 10px 0.25em 2ex 20%;}

padding属性是一个复合属性，包含了4个子属性，分别是padding-top, padding-right, padding-bottom和padding-left，用来设置上，右，下和左边的内边距。例如，上面简写形式与下面的写法是等价的:
::

	h3 {
		padding-top : 10px;
		padding-right: 0.25em;
		padding-bottom: 2ex;
		padding-left: 20%;
	}

**边框**

页面元素的边框就是围绕元素内容和内边距的一条或多条线。颜色，样式和宽度，这三个方面决定了边框所显示出来的外观。

**外边距**

在框模型中，围绕在元素内容边框以外的空白区域是外边距，通过设置外边距会在元素外创建额外的"空白"。

设置外边距最简单的方法就是使用CSS的margin属性，它的取值与padding一样，可以接受任何长度单位，百分数值甚至负值。

元素定位与布局
^^^^^^^^^^^^

**定位**

CSS中实现元素定位的原理很简单，就是使用有效方法精确地将元素定义到页面的特定位置。这个位置可以是页面的绝对位置，也可以位于其上级元素，还可以是另一个元素或浏览器窗口的相对位置。

CSS中全部的定位属性如下所示:

- position： 定义元素在页面中的定位方式
- left：指定元素与最近一个具有定位设置的父对象左边的距离
- right：指定元素与最近一个具有定位设置的父对象右边的距离
- Top：指定元素与最近一个具有定位设置的父对象上边的距离
- Bottom：指定元素与最近一个具有定位设置的父对象下边的距离
- z-index：设置元素的层叠顺序，仅在position属性为relative或者absolute时有效
- Width：设置元素框宽度
- Height：设置元素框高度
- Overflow：内容溢出控制
- Clip：剪切

前6个属性是实际的定位属性，后面4个是相关属性，用来设置区块，或对区块中内容进行控制。其中，position属性是最主要的定位属性，它既可以定义区块的绝对位置，又可以定义相对位置，而left, right, top和bottom只有在position属性中使用才会发挥作用。z-index属性用来标识元素的层叠位置，其属性值为整数，属性值越大，顺序就越靠前，能够叠加到属性值小的元素上，从而优先显示。

CSS为position属性提供了4个属性值来实现4种不同的定位类型，分别是absolute(绝对定位), relative(相对定位), static(静态定位)和fixed(固定定位)，默认为static。

- *绝对定位* : 在绝对定位中，区块的显示位置将参照浏览器左上角的0点为开始点，其偏移方向及距离将配合边偏移属性的设定进行定位，并且是浮动于正常区块之上的。定义了绝对定位的区块将固定于页面中的某个区域，而且不会随着页面变化而变化。

- *相对定位* : 相对定位是以上级区块的原始点为参照点，然后再配合偏移属性对区块进行定位。如果区块无上级元素，则以body为参照点；如果上级区块有边距或填充属性，则参照点以上级区块的内容区域为原始点进行定位。

- *静态定位* : 当未指定position属性值或使用static作为值时，会使用静态定位(static)页面中的区块。在这种情况下，区块会按照在页面中定义的先后顺序，按次序出现在页面中而不会发生偏移，也就是无特殊定位，区块遵循HTML的定位规则。

- *固定定位* : 固定定位的参照位置不是上级区块而是浏览器窗口。所以，可以使用固定定位来设定类似传统框架样式布局以及广告框架或导航框架等。使用固定定位的元素可以脱离页面，无论页面如何滚动，始终位于页面的同一位置上。

jQuery
-------

**为什么要用jQuery？**

1) 轻量级

2) 出色的浏览器兼容性

3) 出色的DOM操作

4) 链式操作方式：在对一个(组)对象执行一组操作的时候，可以直接连写，而无需重复地对对象的引用进行操作。这样能够使jQuery程序更加连贯，也显得更加优雅。

5) 隐式的迭代集合

6) 行为层与结构的分离：

页面DOM元素和JavaScript的主动结合主要是靠DOM元素的事件。传统的事件绑定方式通常是在元素的标签中添加相应的属性。例如，要为页面上的一个命令按钮添加一个单击事件，通常会这样修改该按钮的标签
::

	<button id="myId" onclick="javascript:myId_click()">Command</button>

当然，myId_click()是JavaScript代码中已经编写好的一个函数。

然后，在jQuery中可以使用更加结构化的方式，不需对DOM元素做任何代码上的修改。代码如下：
::

	$("#myId").click(function(){
		// 这里是函数内容
	});

7) 支持扩展

8) 完善的学习资源

**jQuery核心函数**

在jQuery中，所有的DOM对象都将封装成jQuery对象，而且只有jQuery对象才能使用jQuery的方法或属性来执行相应的操作。所以，jQuery提供了一个可以将DOM对象封装成jQuery对象的函数，就是jQuery核心函数jQuery()，也称为工厂函数。

jQuery核心函数有7个重载，分别如下：

(1). jQuery()：在jQuery1.4以后的版本中，该函数返回一个空jQuery对象；在jQuery1.4以前的版本中，该函数会返回一个包含document节点的对象。

(2). jQuery(elements)：该函数将一个或多个DOM元素转化为jQuery对象(或jQuery集合)。另外，这个函数也可以把XML文档和Window对象作为有效的参数。

(3). jQuery(callback)：该函数是jQuery(document).ready(callback)的简写。该函数将绑定一个在DOM文档载入完成后执行的函数。页面中所有需要在DOM加载完时执行的jQuery操作，都需要包含在这个函数中。

(4). jQuery(expression,[context])：该函数接收一个包含jQuery选择器的字符串，然后用这个字符串去匹配一个或多个元素。

(5). jQuery(html)：该函数根据提供的HTML标记代码，动态创建由jQuery对象封装的DOM元素，代码如下：
::

	jQuery("<div></div>")

(6). jQuery(html, props)：该函数根据提供的HTML标记代码，动态创建由jQuery对象封装的DOM元素，同时对该DOM元素设置一组属性，事件等，代码如下：
::

	jQuery("<input>",{type: "text", name="username"})

(7). jQuery(html, [ownerDocument])：该函数根据提供的HTML标记代码，动态创建由jQuery对象封装的DOM元素，并且指定该DOM元素所在的文档。

另外，jQuery核心函数有另一个非常简单的别名$(美元符号，英文为dollar)。

需要注意的是，jQuery对象并不是普通对象，所以一般的变量无法对其直接引用，像下面这样使用就会报错:
::

	var obj = jquery();

不过，只要在变量名前面加一个符号$，就没有错误了。代码如下:
::

	var $obj = jquery();

jQuery选择器
^^^^^^^^^^^^^

分为基本选择器和过滤选择器，并且配合使用，组合成一个选择器字符串，主要的区别是过滤选择器是指定条件从前面匹配的内容中筛选。过滤选择器也可以单独使用，表示从全部的"*"中筛选。

基本选择器包括CSS选择器，层级选择器和表单域选择器。

**CSS选择器**

jQuery借用了一套CSS选择器，共有5种，即标签选择器，ID选择器，类选择器，通用选择器，群组选择器。

**表单域选择器**

表单域就是指网页中的input,textarea,select和button元素。其中，input元素可以具有各种各样的type属性值，如"text", "password","radio"以及"checkbox"等。jQuery提供了一组选择器，专门用于从文档中选择表单域。这些选择器均以冒号(:)开头，其中多数都可以独立使用。

**过滤选择器**

使用基本选择器在文档中进行查询的时候，jQuery对象通常会包含一组DOM元素。在实际应用中，往往还要根据特定条件从获取的元素集合中筛选一部分DOM元素。在这种情况下，可以在基本选择器的基础上添加过滤选择器来完成查询任务。根据具体情况，在过滤选择器中可以使用元素的索引值，内容，属性，子元素位置，表单域属性以及可见性作为筛选条件。

操作jQuery集合
^^^^^^^^^^^^^^

使用选择器从文档中选择一组DOM元素后将得到一个jQuery对象。在某些情况下，还需要从这个jQuery对象包装的元素集合中进一步筛选部分元素进行操作。在这种情况下，可以对该jQuery对象执行相关的筛选方法，以便从匹配元素中筛选出符合条件的DOM元素。
::

	搜索操作	---> 搜索父元素
		  ---> 搜索同辈元素
		  ---> 搜索子元素
	串联操作
	过滤操作

jQuery中的DOM操作
^^^^^^^^^^^^^^^^^

**jQuery中基本的DOM操作**

*查找节点*

- 查找元素节点：查找HTML文档中的元素节点，可以通过jQuery中的选择器来完成。
::

	$("element");		//标签选择器
	$("#id");			//id选择器
	$(".class");		//类选择器
	$("*");				//通用选择器

- 查找文本节点：查找HTML文档中的文本节点，可以通过一个元素节点的text()方法来获得。
::

	$("#id").text();

- 查找属性节点：查找HTML文档中的属性节点，可以通过一个元素节点的attr()方法来获得。
::

	$("#id").attr(name);

*创建节点*

在jQuery中，创建节点可以通过工厂函数$()来完成。

*删除节点*

在对一个HTML文档中的节点进行操作的过程中，页面中难免会有些节点是我们不想要的，此时应该将其删除。对于删除节点，jQuery提供了3种方法，即remove()，detach()和empty()。

*复制节点*

*替换节点*

替换节点，顾名思义就是拿一个或一类节点替换掉另外一个或一类节点。在jQuery中，完成这个功能的方法有两个，分别是replaceAll()和replaceWith()。

- replaceAll()：用指定的节点替换掉所有selector选择器匹配到的节点。其语法格式如下:
::

	$(content).replaceAll(selector);

其中，参数content是必选的，指定用来替换的节点内容，其内容可以是元素节点，HTML字符串或者jQuery对象；参数selector也是必选的，通过该参数匹配被替换的节点内容。

- replaceWith()：将所有selector选择器匹配的节点替换成指定的节点，其语法格式如下:
::

	$(selector).replaceWith(content);

事实上，这个方法的效果和replaceAll()方法完全相同。

**内部插入**

使用工厂函数创建的节点并不能马上动态地显示到网页中，还需要有一个节点对象到DOM文档树中的过程。关于如何向DOM文档树中插入节点，jQuery提供了很多方法，有些在节点内部插入，有的在节点外部插入，有的在节点之前插入，有的在节点之后插入。

*append()方法* ：在指定节点的尾部插入指定内容，语法格式如下

语法1
::

	$(selector).append(content);

语法2
::

	$(selector).append(function(index, [html]));

*appendTo()方法* ：语法格式如下
::

	$(content).appendTo(selector);

事实上，这个方法的效果和append()方法完全相同。

*prepend()方法* ：在指定节点的头部插入指定内容。语法格式如下

语法1
::

	$(selector).prepend(content);

语法2
::

	$(selector).prepend(function(index, [html]));

*prependTo()方法*

**外部插入**

