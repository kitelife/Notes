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

