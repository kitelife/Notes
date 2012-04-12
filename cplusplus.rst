C++语言学习
==========

变量和基本类型
-------------

C++支持两种初始化变量的形式: *复制初始化*(copy-initialization)和*直接初始化*(direct-initialization)。复制初始化语法用等号(=)，直接初始化则是把初始化式放在括号中:
::
	int ival(1024);		// direct-initialization
	int ival = 1024;		// copy-initialization

初始化内置类型的对象只有一种方法: 提供一个值，并且把这个值复制到新定义的对象中。对内置对象来说，复制初始化和直接初始化几乎没有差别。

**变量初始化规则**

当定义没有初始化式的变量时，系统有时候会帮我们初始化变量。这时，系统提供什么样的值取决于变量的类型，也取决于变量定义的位置。

- 内置类型变量的初始化

内置类型变量是否自动初始化取决于变量定义的位置。在函数体外定义的变量都初始化成0，在函数体里定义的内置类型变量不进行自动初始化。除了用作赋值操作符的左操作数，未初始化变量用作任何其他用途都是没有定义的。

- 类类型变量的初始化

每个类都定义了该类型的对象可以怎样初始化。类通过定义一个或多个构造函数来控制类对象的初始化。

如果定义某个类的变量时没有提供初始化式，这个类也可以定义初始化时的操作。它是通过定义一个特殊的构造函数即**默认构造函数**(default constructor)来实现的。

**声明和定义**

变量的**定义**(definition)用于为变量分配存储空间，还可以为变量指定初始值。在一个程序中，变量的定义有且仅有一个定义。

**声明**(declaration)用于向程序表明变量的类型和名字。定义也是声明: 当定义变量时我们声明了它的类型和名字。可以通过使用extern关键字声明变量而不定义它。不定义变量的声明包括对象名，对象类型和对象类型前的关键字extern:
::
	extern int i;		// declares but does not define i
	int i;				// declares and defines i

extern声明不是定义，也不分配存储空间。事实上，它只是说明变量定义在程序的其他地方。程序中变量可以声明多次，但只能定义一次。

*如果声明有初始化式，那么它可被当作是定义，即使声明标记为extern：
::
	extern double pi = 3.1416;		// definition

只有当extern声明位于函数外部时，才可以含有初始化式。

**const限定符**

const对象默认为文件的局部变量。

*非const变量默认为extern。要使const变量能够在其他的文件中访问，必须显式地指定它为extern。*

**引用**

引用(reference)就是对象的另一个名字。在实际程序中，引用主要用作函数的形式参数。

引用是一种复合类型(compound type)。通过在变量名前添加"&"符号来定义。复合类型是指用其他类型定义的类型。在引用的情况下，每一种引用类型都"关联到"某一其他类型。不能定义引用类型的引用，但可以定义任何其他类型的引用。

引用必须用与该引用同类型的对象进行初始化:
::
	int ival = 1024;
	int &refVal = ival;	// ok: refVal refers to ival
	int &refVal2;			// error: a reference must be initialized
	int &refVal3 = 10;		// error: initializer must be an object

- 引用是别名：因为引用只是它绑定的对象的另一个名字，作用在引用上的所有操作事实上都是作用在该引用绑定的对象上。

- const引用：const**引用**是指向const对象的引用
::
	const int ival = 1024;
	const int &refVal = ival;		// ok: both reference and object are const
	int &ref2 = ival;					// error: nonconst reference to a const object

**typedef**

typedef通常被用于以下三种目的:

- 为了隐藏特定类型的实现，强调使用类型的目的
	
- 简化复杂的类型定义，使其更易理解

- 允许一种类型用于多个目的，同时使得每次使用该类型的目的明确。

**类类型**

用class和struct关键字定义类的唯一差别在于默认访问级别：默认情况下，struct的成员为public，而class的成员为private。

**编写自己的头文件**

- 头文件用于声明而不是用于定义：对于头文件不应该含有定义这一规则，有三个例外，头文件可以定义类，值在编译时就已知道的const对象和inline函数。 

标准库类型
---------

标准库string类型
^^^^^^^^^^^^^^^^^^^

**string对象的定义和初始化**

- string s1;				// 调用默认构造函数，s1为空串

- string s2(s1);			// 将s2初始化为s1的一个副本

- string s3("value");		// 将s3初始化为一个字符串字面值副本

- string s4(n, 'c');		// 将s4初始化为字符'c'的n个副本

**string对象的读写**

string类型的输入:

- 读取并忽略开头所有的空白字符(如空格，换行符，制表符)

- 读取字符直至再次遇到空白字符，读取终止。

*读入未知数目的string对象* :
::
	#include <iostream>
	#include <string>
	using namespace std;

	int main()
	{
		string word;
		// read until end-of-file, writing each word to a new line
		while(cin >> word)
			cout << word << endl;

		return 0;
	}

*用getline读取整行文本*

另外还有一个有用的string IO操作: **getline** 。这个函数接受两个参数：一个输入流对象和一个string对象。getline函数从输入流的下一行读取，并保存读取的内容到string对象中，但不包括换行符。和标准输入操作符不一样的是，getline并不忽略行开头的换行符。只要getline遇到换行符，即便它是输入的第一个字符，getline也将停止读取并返回。如果第一个字符就是换行符，则string参数将被置为空string。

getline函数将istream参数作为返回值，和标准输入操作符一样也把它用作判断条件。
::
	#include <iostream>
	#include <string>
	using namespace std;

	int main()
	{
		string line;
		// read line at time until end-of-file
		while(getline(cin, line))
			cout << line << endl;

		return 0;
	}

**string对象的操作**

当进行string对象和字符串字面值混合连接操作时，+操作符的左右操作数必须至少有一个是string类型的。

标准库vector类型
^^^^^^^^^^^^^^^^^^^

vector是同一种类型的对象的集合。每个对象都有一个对应的整数索引值。

vector的下标操作只能用于获取已存在的元素。

迭代器简介
^^^^^^^^^^^^

除了使用下标来访问vector对象的元素外，标准库还提供了另一种访问元素的方法：使用**迭代器**(iterator)。迭代器是一种检查容器内元素并遍历元素的数据类型。

其他
----

- endl是一个特殊值，称为**操纵符(manipulator)**，将它写入输出流时，具有输出换行的效果，并刷新与设备相关联的缓冲区。通过刷新缓冲区，用户可立即看到写入到流中的输出。

- *在写C++程序时，大部分出现空格符的地方可用换行符代替。这条规则的一个例外是字符串字面值中的空格符不能用换行符代替。另一个例外是空格符不允许出现在预处理指示中。*
- 读入未知数目的输入:
::
	#include <iostream>
	using namespace std;
	
	int main()
	{
		int sum = 0, value;
		// read till end-of-file, calculating a running total of all values read
		while(cin >> value)
			sum += value;

		cout << "Sum is: " << sum << endl;
		return 0;
	}

- 通常把一个对象定义在它首次使用的地方是一个很好的方法，这样可以提高程序的可读性。

- 使用const限定符可以把一个变量定义为一个常量。因为常量在定义后就不能被修改，所以定义时必须初始化:
::
	const string hi = "hello!";		// ok: initialized
	const int i, j = 0;				// error: i is uninitialized const

