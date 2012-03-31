## Python魔术方法

### 构造与初始化

每个人都知道一个最基本的魔术方法，\_\_init\_\_。通过此方法可以定义一个对象的初始操作。然而，当我们调用x = SomeClass()的时候，\_\_init\_\_并不是第一个被调用的方法。实际上，还有一个叫做\_\_new\_\_的方法，来构造这个实例。然后给在开始创建时候的初始化函数传递参数。在对象生命周期的另一端，也有一个\_\_del\_\_的方法。

* \_\_new\_\_(cls,[...])

\_\_new\_\_是在一个对象实例化的时候所调用的第一个方法。它的第一个参数是这个类，其他的参数是用来直接传递给\_\_init\_\_方法。

* \_\_init\_\_(self,[...])

此方法为类的初始化方法。当构造函数被调用的时候的任何参数都将会传给它。(比如，如果我们调用x = SomeClass(10,'foo')，那么\_\_init\_\_将会得到两个参数10和foo。)\_\init\_\_在Python的类定义中被广泛用到。

* \_\_del\_\_

如果\_\_new\_\_和\_\_init\_\_是对象的构造器的花，那么\_\_del\_\_就是析构器。它不实现语句del x(所以代码将不会翻译为x.\_\_del\_\_()。del x由\_\_delitem\_\_(...)实现)。它定义的是当一个对象进行垃圾回收时候的行为。当一个对象在删除的时候需要更多的清洁工作的时候此方法会很有用，比如套接字对象或者是文件对象。注意，因为当解释器退出的时候如果对象还存在，不能保证\_\_del\_\_能够被执行，所以\_\_del\_\_ can't serve as a replacement for good coding practice.

### 用于比较的魔术方法

**\_\_cmp\_\_(self, other)**: \_\_cmp\_\_是最基本的用于比较的魔术方法。它实际上实现了所有的比较符号(<,==,!=,etc.)，但是它的表现并不会总是如你所愿(比如，当一个实例与另一个实例相等是通过一个规则来判断的，而一个实例大于另一个实例是通过另一个规则来判断)。通常最好的一种方式是去分别定义每一个比较符号而不是一次性将它们都定义。但是\_\_cmp\_\_方法是你想要实现所有的比较符号而一个保持清楚明白的一个好的方法。

\_\_eq\_\_(self, other)定义了等号的行为,==

\_\_ne\_\_(self, other)定义了不等号的行为，!=

\_\_lt\_\_(self, other)定义了小于号的行为，<

\_\_gt\_\_(self, other)定义了大于等于号的行为，>=

### 数值处理的魔术方法

* 一元操作符和函数

仅仅有一个操作数的一元操作符和函数，比如绝对值，负等。

\_\_pos\_\_(self)实现正号的特性(比如+some_object)

\_\_neg\_\_(self)实现负号的特性(比如-some_object)

\_\_abs\_\_(self)实现内置abs()函数的特性

\_\_invert\_\_(self)实现~符号的特性。

* 普通算数操作符

\_\_add\_\_(self, other)实现方法。

\_\_sub\_\_(self, other)实现减法。

\_\_mul\_\_(self, other)实现乘法。

\_\_floordiv\_\_(self, other)实现//符号实现的整数除法

\_\_div\_\_(self, other)实现/符号实现的除法

\_\_truediv\_\_(self, other) 实现真除法。注意只有只用了 from __future__ import division 的时候才会起作用

\_\_mod\_\_(self, other) 实现取模算法 % 

\_\_divmod\_\_(self, other) 实现内置divmod()算法

\_\_pow\_\_(self, other)实现使用 ** 的指数运算

\_\_lshift\_\_(self, other)实现使用 << 的按位左移动

\_\_rshift\_\_(self, other) 实现使用 >> 的按位左移动

\_\_and\_\_(self, other)实现使用 & 的按位与

\_\_or\_\_(self, other) 实现使用 | 的按位或

\_\_xor\_\_(self, other) 实现使用 ^ 的按位异或

* 增量赋值

Python也有大量的魔术方法可以来定制增量赋值语句。你也许对增量赋值已经很熟悉，它将操作符与赋值结合起来。如果你仍然不清楚我在说什么的话，这里有一个例子:

	x = 5
	x += 1	# in other words x = x + 1
	
\_\_iadd\_\_(self, other)实现赋值加法

\_\_isub\_\_(self, other)实现赋值减法

\_\_imul\_\_(self, other)实现赋值乘法

\_\_ifloordiv\_\_(self, other)实现 //= 的赋值整数除

\_\_idiv\_\_(self, other)实现符号 /= 的赋值除

\_\_itruediv\_\_(self, other)实现赋值真除,只有使用 from __future__ import division 的时候才能使用

\_\_imod\_\_(self, other)实现符号 %= 的赋值取模

\_\_ipow\_\_(self, other)实现符号 **= 的赋值幂运算

\_\_ilshift__(self, other)实现符号 <<= 的赋值位左移

\_\_irshift\_\_(self, other)实现符号 >>= 的赋值位右移

\_\_iand\_\_(self, other)实现符号 &= 的赋值位与

\_\_ior\_\_(self, other) 实现符号 |= 的赋值位或

\_\_ixor\_\_(self, other)实现符号 |= 的赋值位异或

* 类型转换魔术方法

### 表现你的类

如果有一个字符串来表示一个类，那将会很有用。在Python中，有很多方法可以实现类定义内置的一些函数的返回值。

\_\_str\_\_(self)定义当str()调用的时候的返回值

\_\_repr\_\_(self)定义repr()被调用的时候的返回值。

str()和repr()的主要区别在于repr()返回的是机器可读的输出，而str()返回的是人类可读的。

\_\_unicode\_\_(self)定义当unicode()调用的时候的返回值

unicode()和str()很相似，但是返回的是unicode字符串。注意，如果对你的类调用str()而你只定义了\_\_unicode\_\_()，那么将不会工作。你应该定义\_\_str\_\_()来确保调用时返回正确的值。

\_\_hash\_\_(self)定义当hash()调用的时候的返回值，它返回一个整型，用来在字典中进行快速比较。

\_\_nonzero\_\_(self)定义当bool()调用的时候的返回值。本方法应该返回True或者False，取决于你想让它返回的值。

### 控制属性访问

许多从其他语言转到Python的人会抱怨它缺乏类的真正封装。(没有办法定义私有变量，然后定义公共的getter和setter)。Python其实可以通过魔术方法来完成封装。:

###参考

中文链接: [Python魔术方法指南](http://pycoders-weekly-chinese.readthedocs.org/en/latest/issue6/a-guide-to-pythons-magic-methods.html)

英文链接: [A Guide to Python's Magic Methods](http://www.rafekettler.com/magicmethods.html)

