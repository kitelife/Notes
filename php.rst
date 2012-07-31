代码重用
=========

PHP提供了两个非常简单却很有用的语句，它们允许重新使用任何类型的代码。使用一条require()或include()语句，可以将一个文件载入到PHP脚本中。通常，这个文件可以包含任何希望在一个脚本中输入的内容，其中包括PHP语句，文本，HTML标记，PHP函数或PHP类。

**文件扩展名和require语句**

PHP并不会查看所需文件的扩展名。这就意味着，只要不想直接调用这个文件，就可以任意命名该文件。当使用require()语句载入文件时，它会作为PHP文件的一部分被执行。

通常，如果PHP语句放在一个HTML文件(例如，名为page.html的文件)时，它们是不会被处理的。PHP通常用来解析扩展名被定义为.php的文件。但是，如果通过require()语句载入这个page.html，文件内的任何PHP命令都会被处理。因此，可以使用任何扩展名来命名包含文件，但要尽量遵循一个约定，例如将扩展名命名为.inc是一个很好的办法。需要注意的一个问题是，如果扩展名为.inc或一些其他的非标准扩展名的文件保存在Web文档树中，而且用户可以在浏览器中直接载入它们，用户将可以以普通文本的形式查看源代码，因此，将被包含文件保存在文档树之外，或使用标准的文件扩展名是非常重要的。

require()语句和include()语句几乎是等价的。二者的差异在于，当这两个语句调用失败后，require()将给出一个致命错误，而include()只是给出一个警告。

**使用require_once()和include_once()**

require()和include()都有变体函数，分别是require_once()和include_once()，它们的用途是确保一个被包含文件只能被包含一次

类与对象
========

PHP当中对象是按引用传递的，即每个包含对象的变量都持有对象的引用(reference)，而不是整个对象的拷贝。

PHP的引用就是别名，就是两个不同的变量名字指向相同的内容。在php5，一个对象变量已经不再保存整个对象的值，只是保存一个标识符来访问真正的对象内容。当对象作为参数传递，作为结果返回，或者赋值给另外一个变量，另外一个变量跟原来的不是引用的关系，只是他们都保存着同一个标识符的拷贝，这个标识符指向同一个对象的真正内容。

::

	<?php
	class A {
		public $foo = 1;	
	}

	$a = new A;
	$b = $a;	// $a, $b都是同一个标识符的拷贝

	$b->foo = 2;
	echo $a->foo."\n";

	$c = new A;
	$d = $c;	// $c, $d是引用

	$d->foo = 2;
	echo $c->foo."\n";

	$e = new A;
	
	function foo($obj) {
		$obj->foo = 2;
	}

	foo($e);
	echo $e->foo."\n";
	
	?>

一个类可以在声明中用extends关键字继承另一个类的方法和成员。不能扩展多个类，只能继承一个基类。

被继承的方法和成员可以通过用同样的名字重新声明被覆盖，除非父类定义方法时使用了final关键字。可以通过parent::来访问被覆盖的方法或成员。

::

	<?php
	class SimpleClass
	{
		// 成员声明
		public $var = 'a default value';

		// 方法声明
		public function displayVar() {
			echo $this->var;
		}
	}

	class ExtendClass extends SimpleClass
	{
		// Redefine the parent method
		function displayVar()
		{
			echo "Extending class\n"
			parent::displayVar();
		}
	}
	
	$extended = new ExtendClass();
	$extended->displayVar();
	?>

类的变量成员叫做"属性"，或者"字段"，"特征"。属性声明是由关键字public或者protected或者private开头，然后跟一个变量来组成。属性中的变量可以初始化，但是初始化的值必须是常数，这里的常数是指php脚本在编译阶段时就为常数，而不是编译阶段之后在运行阶段运算出的常数。

在类的成员方法里面，可以通过$this->property (property是属性名字)这种方式来访问类的属性，方法，但是要访问类的静态属性或者在静态方法里面却不能使用，而是使用self::$property。

static关键字
^^^^^^^^^^^^

声明类成员或方法为static，就可以不实例化类而直接访问。不能通过一个对象来访问其中的静态成员(静态方法除外)。

为了兼容PHP4，如果没有指定"可见性",属性和方法默认为public。

由于静态方法不需要通过对象即可调用，所以伪变量$this在静态方法中不可用。

静态属性不可以由对象通过->操作符来访问。

就像其他所有的PHP静态变量一样，静态属性只能被初始化为一个字符值或一个常量，不能使用表达式。所以你可以把静态属性初始化为整型或数组，但不能指向另一个变量或函数返回值，也不能指向一个对象。

::

	<?php
	class Foo
	{
		public static $my_static = 'foo';
		
		public function staticValue() {
			return self::$my_static;		
		}
	}

	class Bar extends Foo
	{
		public function fooStatic() {
			return parent::$my_static;
		}
	}

	print Foo::$my_static . "\n";
	
	$foo = new Foo();
	print $foo->staticValue() . "\n";
	?>

类常量
^^^^^^^

可以在类中使用const关键字定义常量。常量的值始终保持不变。在定义和使用常量的时候不需要使用$符号。

常量的值必须是一个定值，不能是变量，类属性或其他操作(如函数调用)的结果。

PHP5.3之后，可以用一个变量来动态调用类。但该变量的值不能为关键字self, parent或static。

::

	<?php
	class MyClass
	{
		const constant = 'constant value';
		
		function showContent() {
			echo self::constant . "\n";
		}
	}

	echo MyClass::constant . "\n";

	$classname = "MyClass";
	echo $classname::constant . "\n";	// PHP 5.3之后

	$class = newMyClass();
	$class->showConstant();

	echo $class::constant . "\n"
	?>


自动加载对象
^^^^^^^^^^^^

很多开发者写面向对象的应用程序时对每个类的定义建立一个PHP源文件。一个很大的烦恼是不得不在每个脚本(每个类一个文件)开头写一个长长的的包含文件列表。

在PHP5中，不再需要这样了。可以定义一个__autoload函数，它会在试图使用尚未定义的类时自动调用。通过调用此函数，脚本引擎在PHP出错失败前有了最后一个机会加载所需的类。

*在__autoload函数中抛出的异常不能被catch语句块捕获并导致致命错误。*

::

	<?php
	function __autoload($class_name) {
		require_once $class_name . '.php';
	}

	$obj = new MyClass1();
	$obj2 = new MyClass2();
	?>

本例尝试分别从MyClass1.php和MyClass2.php文件中加载MyClass1和MyClass2类。

构造函数和析构函数
^^^^^^^^^^^^^^^^^

抽象类
^^^^^^^^

PHP5支持抽象类和抽象方法。抽象类不能直接实例化，必须继承该抽象类，然后再实例化子类。抽象类中至少要包含一个抽象方法。如果类方法被声明为抽象的，那么其中就不能包括具体的功能实现。

继承一个抽象类的时候，子类必须实现抽象类中的所有抽象方法；另外，这些方法的可见性必须和抽象类中一样(或者更为宽松)。如果抽象类中某个抽象方法被声明为protected，那么子类中实现的方法就应该声明为protected或者public，而不能定义为private。

::

	<?php
	abstract class AbstractClass
	{
		// 强制要求子类定义这些方法
		abstract protected function getValue();
		abstract protected function prefixValue($prefix);

		// 普通方法(非抽象方法)
		public function printOut() {
			print $this->getValue() . "\n";
		}
	}

	class ConcreteClass1 extends AbstractClass
	{
		protected function getValue()
		{
			return "ConcreteClass1";
		}
		
		public function prefixValue($prefix) {
			return "{$prefix}ConcreteClass1";		
		}
	}

	class ConcreteClass2 extends AbstractClass
	{
		public function getValue() {
			return "ConcreteClass2";
		}

		public function prefixValue($prefix) {
			return "{$prefix}ConcreteClass2";
		}
	}

	$class1 = new ConcreteClass1;
	$class1->printOut();
	echo $class1->prefixValue('FOO_') . "\n";

	$class2 = new ConcreteClass2;
	$class2->printOut();
	echo $class2->prefixValue('FOO_') . "\n";
	?>

接口
^^^^^

使用接口(interface)，可以指定某个类必须实现哪些方法，但不需要定义这些方法的具体内容。

可以通过interface来定义一个接口，就像定义一个标准的类一样，但其中定义所有的方法都是空的。

接口中定义的所有方法都必须是public，这是接口的特性。

要实现一个接口，可以使用implements操作符。类中必须实现接口中定义的所有方法，否则会报一个fatal错误。如果要实现多个接口，可以用逗号来分隔多个接口的名称。

*实现多个接口时，接口中的方法不能重名。*

*接口也可以继承，通过使用extends操作符。*

接口中也可以定义常量。接口常量和类常量的使用完全相同。它们都是定值，不能被子类或子接口修改。

重载
^^^^^

PHP所提供的"重载"(overloading)是指动态地"创建"类属性和方法。通过魔术方法(magic methods)来实现的。

所有的重载方法都必须被声明为public。

对象迭代
^^^^^^^^^^

PHP5提供了一种迭代(iteration)对象的功能，就像使用数组那样，可以通过foreach来遍历对象中的属。默认情况下，在外部迭代只能得到外部可见的属性的值。

::

    <?php
    class MyClass
    {
        public $var1 = 'value 1';
        public $var2 = 'value 2';
        public $var3 = 'value 3';

        protected $protected = 'protected var';
        private $private = 'private var';

        function iterateVisible() {
            echo "MyClass::iterateVisible:\n";
            foreach($this as $key => $value) {
                print "$key => $value\n";
            }
        }
    }

    $class = new MyClass();

    foreach($class as $key = > $value) {
        print "$key => $value\n";
    }

    echo "\n";

    $class->iterateVisible();
    ?>

如上所示，foreach遍历了所有可见的属性。你也可以通过实现PHP5自带的Iterator接口来实现迭代。使用Iterator接口可以让对象自行决定如何迭代自己。

::

    <?php
    class MyIterator implements Iterator
    {
        private $var = array();

        public function __construct($array)
        {
            if (is_array($array)) {
                $this->var = $array;
            }
        }

        public function rewind() {
            echo "rewinding\n";
            reset($this->var);
        }

        public function current() {
            $var = current($this->var);
            echo "current: $var\n";
            return $var;
        }

        public function key() {
            $var = key($this->var);
            echo "key: $var\n";
            return $var;
        }

        public function valid() {
            $var = $this->current() != false;
            echo "valid: {$var}\n";
            return $var;
        }
    }

    $values = array(1, 2, 3);
    $it = new MyIterator($values);

    foreach ($it as $a => $b) {
        print "$a: $b\n";
    }
    ?>

设计模式
^^^^^^^^^

**工厂模式**

工厂模式(Factory)允许你在代码执行时实例化对象。它之所以被称为工厂模式是因为它负责"生产"对象。工厂方法的参数是你要生成的对象对应的名称。

::

    <?php
    class Example
    {
        // The parameterized factory method
        public static function factory($type)
        {
            if (include_once 'Drivers/' . $type . '.php') {
                $classname = 'Driver_' . $type;
            } else {
                throw new Exception ('Driver not found');
            }
        }
    }
    ?>

按上面的方式可以动态加载drivers。如果Example类是一个数据库抽象类，那么可以这样来生成MySQLhe SQLite驱动对象。

::

    <?php
    // Load a MySQL Driver
    $mysql = Example::factory('MySQL');

    // Load a SQLite Driver
    $sqlite = Example::factory('SQLite');
    ?>

**单例**

单例模式(Singleton)用于为一个类生成一个唯一的对象。最常用的地方是数据库连接。使用单例模式生成一个对象后，该对象可以被其它众多对象所使用。

::

    <?php
    class Example
    {
        // 保存类实例在此属性中
        private static $instance;

        // 构造方法声明为private，防止直接创建对象
        private function __construct()
        {
            echo 'I am constructed';
        }

        // singleton方法
        public static function singleton()
        {
            if (!isset(self::$instance)) {
                $c = __CLASS__;
                self::$instance = new $c;
            }

            return self::$instance;
        }

        // Example类的普通方法
        public function bark()
        {
            echo 'Woof!';
        }

        // 阻止用户复制对象实例
        public function __clone()
        {
            trigger_error('Clone is not allowed.', E_USER_ERROR);
        }
    }
    ?>

这样我们就能得到一个独一无二的Example类的对象。

::

    <?php

    // 这个写法会出错，因为构造方法被声明为private
    $test = new Example;

    // 下面将得到Example类的单例对象
    $test = Example::singleton();
    $test->bark();

    // 复制对象将导致一个E_USER_ERROR.
    $test_clone = clone $test;
    ?>

Final关键字
^^^^^^^^^^^^

PHP5新增了一个final关键字。如果父类中的方法被声明为final，则子类无法覆盖该方法；如果一个类被声明为final，则不能被继承。

对象复制
^^^^^^^^^

在多数情况下，我们并不需要完全复制一个对象来获得其中属性。但有一个情况下确实需要：如果你有一个GTK窗口对象，该对象持有窗口相关的资源。你可能会复制一个新的窗口，保持所有属性与原来的窗口相同，但必须是一个新的对象(因为如果不是新的对象，那么一个窗口中的改变就会影响到另一个窗口)。还有一种情况：如果对象A中保存着对象B的引用，当你复制对象A时，你想其中使用的对象不再是对象B而是对象B的一个副本，那么你必须得到对象A的一个副本。

对象复制可以通过clone关键字来完成(如果可能，这将调用对象的__clone()方法)。对象中的__clone()方法不能被直接调用。

::

    $copy_of_object = clone $object;

当对象被复制后，PHP5会对对象的所有属性执行一个浅复制(shallow copy)。所有的引用属性仍然会是一个指向原来的变量的引用。

对象比较
^^^^^^^^^^

当使用对比操作符(==)比较两个对象变量时，比较的原则是：如果两个对象的属性和属性值都相等，而且两个对象是同一个类的实例，那么这两个对象变量相等。

而如果使用全等操作符(===)，这两个对象变量一定要指向某个类的同一个实例(即同一个对象)。

字符串的连接
^^^^^^^^^^^^^^

字符串连接符---点号(.)，可以将几段文本连接成一个字符串。通常，当使用echo命令向浏览器发送输出时，将使用这个连接符。这可以用来避免编写多个echo命令。

对于任何非数组变量，也可以将变量写入到一个由双引号引用起来的字符串中：

::

    echo "$tireqty tires<br />";

这个语句等价于：

::

    echo $tireqty.' tires<br />';

这两种格式都是有效的，而且使用任何一种格式都只是个人爱好问题。

用一个字符串的内容代替一个变量的操作就是插补(interpolation)。 **请注意，插补操作只是双引号引用的字符串的特性之一。** 不能像这样将一个变量名称放置在一个由单引号引用的字符串中。

::

    echo '$tireqty tires<br />';

该代码将"$tireqty tires<br />"发送给浏览器。

在双引号中，变量名称将被变量值所替代，而在单引号中，变量名称，或者其他任何文件都会不经修改而发送给浏览器。


