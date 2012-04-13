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

*读入未知数目的string对象*
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

标准库为每一种标准容器(包括vector)定义了一种迭代器类型。迭代器类型提供了比下标操作更通用化的方法：所有的标准库容器都定义了相应的迭代器类型，而只有少数的容器支持下标操作。因为迭代器对所有的容器都适用，现代C++程序更倾向于使用迭代器而不是下标操作访问容器元素。

**begin和end操作**

每种容器都定义了一对命名为begin和end的函数，用于返回迭代器。如果容器中有元素的话，由begin返回的迭代器指向第一个元素:
::

	vector<int>::iterator iter = ivec.begin();

由end操作返回的迭代器指向vector的"末端元素的下一个"。通常称为**超出末端迭代器**(off-the-end iterator)，表明它指向了一个不存在的元素。如果vector为空，begin返回的迭代器和end返回的迭代器相同。

*由end操作返回的迭代器并不指向vector中任何实际的元素，相反，它只是起一个哨兵(sentinel)的作用，表示我们已经处理完vector中的所有元素。*

**const_iterator**

每种容器类型还定义了一种名为const_iterator的类型，该类型只能用于读取容器内元素，但不能改变其值。

不要把const_iterator对象与const的iterator对象混淆起来。声明一个const迭代器时，必须初始化迭代器。一旦被初始化后，就不能改变它的值。

标准库bitset类型
^^^^^^^^^^^^^^^^

类似于vector，bitset类是一种类模板；而与vector不一样的是bitset类型对象的区别仅在其长度而不在其类型。在定义bitset时，要明确bitset含有多少位，须在尖括号内给出它的长度值:
::

	bitset<32> bitvec;		// 32 bits, all zero

给出的长度值必须是常量表达式。正如这里给出的，长度值必须定义为整数字面值常量或者已用常量值初始化的整型的const对象。

**用unsigned值初始化bitset对象**

当用unsigned long值作为bitset对象的初始值时，该值将转化为二进制的位模式。而bitset对象中的位集作为这种位模式的副本。如果bitset类型长度大于unsigned long值的二进制位数，则其余的高阶位将置为0；如果bitset类型长度小于unsigned long值的二进制位数，则只使用unsigned值中的低阶位，超过bitset类型长度的高阶位将被丢弃。

**用string对象初始化bitset对象**

当用string对象初始化bitset对象时，string对象直接表示为位模式。从string对象读入位集的顺序是**从右到左**。

*string对象和bitset对象之间是反向转化的：string对象的最右边字符(即下标最大的那个字符)用来初始化bitset对象的最低位(即下标为0的位)。*

数组与指针
----------

现代C++程序应尽量使用vector和迭代器类型，而避免使用低级的数组和指针。设计良好的程序只有在强调速度时才在类实现的内部使用数组和指针。

**指针和引用的比较**

虽然使用引用(reference)和指针都可间接访问另一个值，但它们之间有两个重要区别。第一个区别在于引用总是指向某个对象：定义引用时没有初始化是错误的。第二个重要区别则是赋值行为的差异：给引用赋值修改的是该引用所关联的对象的值，而并不是使引用与另一个对象关联。引用一经初始化，就始终指向同一个特定对象。

**创建动态数组**

虽然数组长度是固定的，但动态分配的数组不必在编译时知道其长度，可以(通常也是)在运行时才确定数组长度。与数组变量不同，动态分配的数组将一直存在，直到程序显示地释放它为止。

C语言程序使用一对标准库函数malloc和free在自由存储区分配存储空间，而C++语言则使用new和delete表达式实现相同的功能。
::

	int *pia = new int[10];			// array of 10 uninitialized ints
	string *psa = new string[10];		// array of 10 empty strings

	delete [] pia;

在关键字delete和指针之间的空方括号对是必不可少的：它告诉编译器该指针指向的是自由存储区中的数组，而并非单个对象。

表达式
-------

箭头操作符
^^^^^^^^^^

C++语言为包含点操作符和解引用操作符的表达式提供了一个同义词：箭头操作符(->)。

点操作符用于获取类类型对象的成员：
::

	item1.same_isbn(item2);			// run the same_isbn member of item1

如果有一个指向Sales_item对象的指针(或迭代器)，则在使用点操作符前，需对该指针(或迭代器)进行解引用:
::

	Sales_item *sp = &item1;
	(*sp).same_isbn(item2);		// run same_isbn on object to which sp points
	sp->same_isbn(item2);		// 与(*sp).same_isbn(item2);等价

new和delete表达式
^^^^^^^^^^^^^^^^^^

除了动态创建和释放数组，new和delete也可用于动态创建和释放单个对象。

动态创建对象时，只需指定其数据类型，而不必为该对象命名。取而代之的是，new表达式返回指向新创建对象的指针，我们通过该指针来访问对象:
::

	int i;		// named, uninitialized int variable
	int *pi = new int;		// pi points to dynamically allocated, unnamed, uninitialized int

**动态创建对象的初始化**

动态创建的对象可用初始化变量的方式实现初始化:
::

	int i(1024);		// value of i is 1024
	int *pi = new int(1024);			// object to which pi points is 1024
	string s(10, '9');				// value of s is "9999999999"
	string *ps = new string(10, '9');		// *ps is "9999999999"

**动态创建对象的默认初始化**

如果不提供显示初始化，动态创建的对象与在**函数内**定义的变量初始化方式相同。对于类类型的对象，用该类的默认构造函数初始化；而内置类型的对象则无初始化。
::

	string *ps = new string;		// initialized to empty string
	int *pi = new int;				// pi points to an uninitialized int

**撤销动态创建的对象**

::

	delete pi;

该命令释放pi指向的int型对象所占用的内存空间。

*如果指针指向不是用new分配的内存地址，则在该指针上使用delete是不合法的。*

**零值指针的删除**

如果指针的值为0，则在其上做delete操作是合法的，但这样做没有任何意义:
::

	int *pi = 0;
	delete pi;		// ok: always ok to delete a pointer that is equal to 0

C++保证：删除0值的指针是安全的。

**在delete之后，重设指针的值**

执行语句:
::

	delete p;

后，p变成没有定义。在很多机器上，尽管p没有定义，但仍然存放了它之前所指向对象的地址，然而p所指向的内存已经被释放，因此p不再有效。

删除指针后，该指针变成**悬垂指针**(dangling pointer)。悬垂指针指向曾经存放对象的内存，但该对象已经不再存在了。悬垂指针往往导致程序错误，而且很难检测出来。

*一旦删除了指针所指向的对象，立即将指针置为0，这样就非常清楚第表明指针不再指向任何对象。*

函数
-----

return语句
^^^^^^^^^^^

函数不能返回另一个函数或者内置数组类型，但可以返回指向函数的指针，或指向数组元素的指针的指针。

**千万不要返回局部对象的引用**

当函数执行完毕时，将释放分配给局部对象的存储空间。此时，对局部对象的引用就会指向不确定的内存。

**千万不要返回指向局部对象的指针**

函数的返回类型可以是大多数类型。特别地，函数也可以返回指针类型。和返回局部对象的引用一样，返回指向局部对象的指针也是错误的，一旦函数结束，局部对象被释放，返回的指针就变成了指向不再存在的对象的悬垂指针。

局部对象
^^^^^^^^

在C++语言中，每个名字都有作用域，而每个对象都有生命期(lifetime)。名字的作用域指的是知道该名字的程序文本区。对象的生命期则是在程序执行过程中对象存在的时间。

**静态局部对象**

一个变量如果位于函数的作用域内，但生命期却跨越了这个函数的多次调用，这种变量往往很有用。则应该将这样的对象定义为static(静态的)。

static局部变量(static local object)确保不迟于在程序执行流程第一次经过该对象定义语句时进行初始化。这种对象一旦被创建，在程序结束前都不会被撤销。当定义静态局部对象的函数结束时，静态局部对象不会撤销。在该函数被多次调用的过程中，静态局部对象会持续存在并保持它的值。
::

	#include <iostream>
	using namespace std;

	size_t count_calls()
	{
		static size_t ctr = 0;		// value will persist across calls
		return ++ctr; 
	}

	int main()
	{
		for(size_t i = 0; i != 10; ++i)
			cout << count_calls() << endl;

		return 0;
	}

这个程序会依次输出1到10(包含10)的整数。

*在第一次调用count_calls之前，ctr就已创建并赋予初值0。* 每次函数调用都使ctr加1，并且返回其当前值。在执行函数count_calls时，变量ctr就已经存在并且保留上次调用该函数时的值。

内联函数
^^^^^^^^^

*内联说明(inline specification)对于编译器来说只是一个建议，编译器可以选择忽略这个建议。*

*内联函数应该在头文件中定义，这一点不同于其他函数。* 内联函数的定义对编译器而言必须是可见的，以便编译器能够在调用点内联展开该函数的代码。此时，仅有函数原型是不够的。

*在头文件中加入或修改内联函数时，使用了该头文件的所有源文件都必须重新编译。*

类的成员函数
^^^^^^^^^^^

编译器隐式地将在类内定义的成员函数当作内联函数。

const对象，指向const对象的指针或引用只能用于调用其const成员函数，如果尝试用它们来调用非const成员函数，则是错误的。

**合成的默认构造函数**

*如果没有为一个类显式定义任何构造函数，编译器将自动为这个类生成默认构造函数。*由编译器创建的默认构造函数通常称为**合成的默认构造函数**(synthesized default constructor)，它将依据如同变量初始化的规则初始化类中所有成员。对于具有类类型的成员，则会调用该成员所属类自身的默认构造函数实现初始化。内置类型成员的初值依赖于对象如何定义。如果对象在全局作用域中定义(即不在任何函数中)或定义为静态局部对象，则这些成员将被初始化为0.如果对象在局部作用域中定义，则这些成员没有初始化。

*合成的默认构造函数一般适用仅包含类类型成员的类。而对于含有内置类型或复合类型成员的类，则通常应该定义他们自己的默认构造函数初始化这些成员。*

重载函数
^^^^^^^

出现在相同作用域中的两个(或多个)函数，如果具有相同的名字而形参表不同，则称为**重载函数**(overloaded function)。

函数重载(function overloading)简化了程序的实现，使程序更容易理解。函数名只是为了帮助编译器判断调用的是哪个函数而已。

**函数重载和重复声明的区别**

如果两个函数声明的返回类型和形参表完全匹配，则将第二个函数声明视为第一个的重复声明。如果两个函数的形参表完全相同，但返回类型不同，则第二个声明是错误的:
::

	Record lookup(const Account&);
	bool lookup(const Account&);		// error: only return type is different

函数不能仅仅基于不同的返回类型而实现重载。

**重载和const形参**

*仅当形参是引用或指针时，形参是否为const才有影响。*

可基于函数的引用形参是指向const对象还是指向非const形参，实现函数重载。将引用形参定义为const来重载函数是合法的，因为编译器可以根据实参是否为const确定调用哪一个函数。

指向函数的指针
^^^^^^^^^^^^^

函数指针是指指向函数而非指向对象的指针。像其他指针一样，函数指针也指向某个特定的类型。函数类型由其返回类型以及形参表确定，而与函数名无关:
::

	// pf points to function returning bool that takes two const string references
	bool (*pf)(const string &, const string &);

*\*pf两侧的圆括号是必须的:*
::

	// declares a function named pf that returns a bool*
	bool *pf(const string &, const string &);

**用typedef简化函数指针的定义**
::

	typedef bool (*cmpFcn)(const string &, const string &);

该定义表示cmpFcn是一种指向函数的指针类型的名字。该指针类型为"指向返回bool类型并带有两个const string引用形参的函数的指针"。在要使用这种函数指针类型时，只需直接使用cmpFcn即可，不必每次都把整个类型声明全部写出来。

**指向函数的指针的初始化和赋值**

在引用函数名但又没有调用该函数时，函数名将被自动解释为指向函数的指针。假设有函数:
::

	// compares lengths of two strings
	bool lengthCompare(const string &, const string &);

除了用作函数调用的左操作数以外，对lengthComapare的任何使用都被解释为如下类型的指针:
::

	bool (*)(const string &, const string &);

可使用函数名对函数指针做初始化或赋值：
::

	cmpFcn pf1 = 0;			// ok: unbound pointer to function
	cmpFcn pf2 = lengthComapre;		// ok: pointer type matches function's type
	pf1 = lengthComapare;			// ok: pointer type matches function's type
	pf2 = pf1;						// ok: pointer type match

此时，直接引用函数名等效于在函数名上应用取地址操作符:
::

	cmpFcn pf1 = lengthCompare;
	cmpFcn pf2 = &lengthCompare;

*函数指针只能通过同类型的函数或函数类型或0值常量表达式进行初始化或赋值。*

标准IO库
--------

.. image:: https://lh3.googleusercontent.com/-k11Sj2bgmHk/T4fspDajmpI/AAAAAAAAA-s/4mBp9TT2I10/s713/cppstdiotree.jpg

其他
----

- endl是一个特殊值，称为**操纵符(manipulator)**，将它写入输出流时，具有输出换行的效果，并刷新与设备相关联的缓冲区。通过刷新缓冲区，用户可立即看到写入到流中的输出。

- *在写C++程序时，大部分出现空格符的地方可用换行符代替。这条规则的一个例外是字符串字面值中的空格符不能用换行符代替。另一个例外是空格符不允许出现在预处理指示中。*
- 读入未知数目的输入
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

- 使用const限定符可以把一个变量定义为一个常量。因为常量在定义后就不能被修改，所以定义时必须初始化
::

	const string hi = "hello!";		// ok: initialized
	const int i, j = 0;				// error: i is uninitialized const
