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

\_\_ipow\_\_(self, other)实现符号 \**= 的赋值幂运算

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

*\_\_getattr\_\_(self,name)*你可以通过它来定义当用户试图获取一个不存在的属性时的行为。这适用于对普通拼写错误的获取和重定向，对获取一些不建议的属性时候给出警告(如果你愿意你也可以计算并且给出一个值)或者处理一个AttributeError。只有当调用不存在的属性的时候会被返回。然而，这不是一个封装的解决方案。

*\_\_setattr\_\_(self,name,value)*与\_\_getattr\_\_不同，\_\_setattr\_\_是一个封装的解决方案。无论属性是否存在，它都允许你定义对属性的赋值行为。但是你必须对使用\_\_setattr\_\_特别小心。

*\_\_delattr\_\_*与\_\_setattr\_\_相同，但是功能是删除一个属性而不是设置它。

### 创建定制的序列

有很多方法让你的Python类行为可以像内置的序列(dict,tuple,list,string等等)。

**Requirements**

Now that we're taking about creating your own sequences in Python, it's time to talk about *protocols*. Protocols are somewhat similar to interfaces in other languages in that they give you a set of methods you must define. However, in Python protocols are totally informal and require no explicit declarations to implement. Rather, they're more like guidelines.

Why are we taking about protocols now? Because implementing custom container types in Python involves using some of these protocols. First, there's the protocol for defining immutable of containers: to make an immutable container, you need only define \_\_len\_\_ and \_\_getitem\_\_(more on these later). The mutable container protocol requires everything that immutable containers require plus \_\_setitem\_\_ and \_\_delitem\_\_. Lastly, if you want your object to be iterable, you'll have to define \_\_iter\_\_, which returns an iterator. That iterator must conform(遵从)to an iterator protocol, which requires iterators to have methods called \_\_iter\_\_(returning itself) and next.

**The magic behind containers**

*\_\_len\_\_(self)*: Returns the length of the container. Part of the protocol for both immutable and mutable containers.

*\_\_getitem\_\_(self, key)*: Defines behavior for when an item is accessed, using the notation *self[key]*. This is also part of both the mutable and immutable containers protocols. It should also raise appropriate exceptions: *TypeError* if the type of the key is wrong and *KeyError* if there is no corresponding value for the key.

*\_\_setitem\_\_(self, key, value)*: Defines behavior for when an item is assigned to, using the notation *self[key] = value*.This is part of the mutable container protocol. Again, you should raise *KeyError* and *TypeError* where appropriate.

*\_\_delitem\_\_(self, key)*: Defines behavior for when an item is deleted(e.g. del self [key]). This is only part of the mutable container protocol. You must raise the appropriate exception when an invalid key is used.

*\_\_iter\_\_(self)*: Should return an iterator for the container. Iterators are returned in a number of contexts, most notably by the *iter()* built in function and when a container is looped over using the form *for x in container*:. Iterators are their own objects, and they also must define an \_\_iter\_\_ method that returns self.

*\_\_reversed\_\_(self)*: Called to implement behavior for the *reversed()* built in function. Should return a reversed version of the list.

*\_\_contains\_\_(self, item)*: defines behavior for membership tests using *in* and *not in*. Why isn't this part of a sequence protocol, you ask? Because when *\_\_contains\_\_* isn't defined, Python just iterates over the sequence and returns *True* if it comes across the item it's looking for.

*\_\_concat\_\_(self, other)*: Lastly, you can define behavior for concatenating your sequence with another by defining *\_\_concat\_\_*. It should return a new sequence constructed from *self* and *other*. *\_\_concat\_\_* is invoked with the *+* operator when it is called on two sequences.

**An example**

	class FunctionalList:
		''' A class wrapping a list with some extra functional magic,
		like head, tail, init, last, drop, and take.
		'''
		def __init__(self, values=None):
			if values is None:
				self.values = []
			else:
				self.values = values

		def __len__(self):
			return len(self.values)

		def __getitem__(self, key):
			# if key is of invalid type or value, 
			# the list values will raise the error
			return self.values[key]

		def __setitem__(self, key, value):
			self.values[key] = value

		def __delitem__(self, key):
			del self.values[key]

		def __iter__(self):
			return iter(self.values)

		def __reversed__(self):
			return reversed(self.values)

		def append(self, value):
			self.values.append(value)

		def head(self):
			# get the first element
			return self.values[0]

		def tail(self):
			# get all elements after the first
			return self.values[1:]

		def init(self):
			# get elements up to the last
			return self.values[:-1]

		def last(self):
			# get last element
			return self.values[-1]

		def drop(self, n):
			# get all elements except first n
			reutrn self.values[n:]

		def take(self, n):
			# get first n elements
			return self.values[:n]

There you have it, a (marginally) useful example of how to implement your own sequence. Of course, there are more useful application of custom sequences, but quite a few of them are already implemented in the standard library (batteries included, right?), like *Counter*, *OrderedDict*, and *NamedTuple*.

### Callable Objects

As you may already know, in Python, functions are first-class objects. This means that they can be passed to functions and methods just as if they were objects of any other kind. This is an incredibly powerful feature.

A special magic method in Python allows instances of your classes to behave as if they were functions, so that you can "call" them, pass them to functions that take functions as arguments, and so on. This is another powerful convenience feature that makes programming in Python that much sweeter.

*\_\_call\_\_(self, [args...])*: Allows an instance of a class to be called as a function. Essentially, this means that *x()* is the same as *x.\_\_call\_\_()*. Note that \_\_call\_\_ takes a variable number of arguments; this means that you define *\_\_call\_\_* as you would any other function, taking however many arguments you'd like it to.

*\_\_call\_\_* can be particularly useful in classes whose instances that need to often change state. "Calling" the instance can be an intuitive and elegant way to change the object's state. An example might be a class representing an entity's position on a plane:

	class Entity:
		'''Class to represent an entity. Callable to update the entity's position.'''
		def __init__(self, size, x, y):
			self.x, self.y = x, y
		
		def __call__(self, x, y):
			'''change the position of the entity'''
			self.x, self.y = x, y

### Context Managers

In Python 2.5, a new keyword was introduced in Python along with a new method for code reuse, the *with* statement. The concept of managers was hardly new in Python (it was implemented before as a part of the library), but not until PEP 343 was accepted did it achieve status as a first class language construct. You may have seen with statements before:

	with open('foo.txt') as bar:
		# perform some action with bar

Context managers allow setup and cleanup actions to be taken for objects when their creation is wrapped with a *with* statement. The behavior of the context manager is determined by two magic methods:

*\_\_enter\_\_(self)*: Defines what the context manager should do at the beginning of the block created by the *with* statement. Note that the return value of *\_\_enter\_\_* is bound to the *target* of the *with* statement, or the name after the as.

*\_\_exit\_\_(self, exception_type, exception_value, traceback)*: Defines what the context manager should do after its block has been executed(or terminates). It can be used to handle exceptions, perform cleanup, or do something always done immediately after the action in the block. If the block executes successfully, *exception_type*, *exception_value*, and *traceback* will be *None*. Otherwise, you can choose to handle the exception or let the user handle it; If you want to handle it, make sure *\_\_exit\_\_* returns *True* after all is said an done. If you don't want the exception to be handled by the context manager, just let it happen.

*\_\_enter\_\_* and *\_\_exit\_\_* can be useful for specific classes that have well-defined and common behavior for setup and cleanup. You can also use these methods to create generic context managers that wrap other objects.

	class Closer:
		''' A context manager to automatically close an object with a close method in a with statement'''
		def __init__(self, obj):
			self.obj = obj

		def __enter__(self):
			return self.obj		# bound to target

		def __exit__(self, exception_type, exception_val, trace):
			try:
				self.obj.close()
			except AttributeError:	# obj isn't closable
				print 'Not closable'
				return True		# exception handled successfully


	>>> from magicmethods import Closer
	>>> from ftplib import FTP
	>>> with Closer(FTP('ftp.somesite.com')) as conn:
	...		conn.dir()
	...
	# output omitted for brevity
	>>> conn.dir()
	# long AttributeError message, can't use a connection that's closed
	>>> with Closer(int(5)) as i:
	...		i += 1
	...
	Not closable
	>>> i
	6

### Pickling Your Objects

If you spend time with other Pythonistas, chances are you've at least heard of pickling. Pickling is a serialization process for Python data structures, and can be incredibly useful when you need to store an object and retrieve it later (usually for caching). It's also a major source of worries and confusion.

Pickling is so important that it doesn't just have its own module (*pickle*), but its own *protocol* and the magic methods to go with it.

	import pickle

	data = {'foo': [1,2,3],
			'bar': ('hello', 'world'),
			'baz': True}
	jar = open('data.pkl', 'wb')
	pickle.dump(data, jar)
	jar.close()

Now, a few hours later, we want it back. All we have to do is unpickle it:

	import pickle

	pkl_file = open('data.pkl', 'rb')
	data = pickle.load(pkl_file)
	print data
	pkl_file.close()

Now, for a word of caution: pickling is not perfect. Pickle files are easily corrupted on accident and on purpose. Pickling may be more secure than using flat text files, but it still can be used to run malicious code. It's also incompatible across versions of Python, so don't expect to distribute pickled objects and expect people to be able to open them. However, it can also be a powerful tool for caching and other common serialization tasks.

**Pickling your own Objects**

Pickling isn't just for built-in types. It's for any class that follows the pickle protocol. The pickle protocol has four optional methods for Python objects to customize how they act.

*\_\_getinitargs\_\_(self)*: If you'd like for \_\_init\_\_ to be called when your class is unpickled, you can define \_\_getinitargs\_\_, which should return a tuple of the arguments that you'd lile to be passed to \_\_init\_\_. Note that this method will only work for old-style classes.

*\_\_getnewargs\_\_(self)*: For new-style classes, you can influence what arguments get passed to \_\_new\_\_ upon unpickling. This method should also return a tuple of arguments that will then be passed to \_\_new\_\_

*\_\_getstate\_\_(self)*: Instead of the object's \_\_dict\_\_ attribute being stored, you can return a custom state to be stored when the object is pickled. That state will be used by *\_\_setstate\_\_* when the object is unpickled.

*\_\_setstate\_\_(self, state)*: When the object is unpickled, if *\_\_setstate\_\_* is defined the object's state will be passed to it instead of directly applied to the object's *\_\_dict\_\_*. This goes hand in hand with *\_\_getstate\_\_*: when both are defined, you can represent the object's pickled state however you want with whatever you want.

**An Example**

Our example is *Slate*, which remember what its values have been and when those values were written to it. However, this particular slate goes blank each time it is pickled: the current value will not be saved.

	import time

	class Slate:
		'''class to store a string and a changelog, and forget its value when pickled'''
		def __init__(self, value):
			self.value = value
			self.last_change = time.asctime()
			self.history = {}

		def change(self, new_value):
			# change the value. Commit last value to history
			self.history[self.last_change] = self.value
			self.value = new_value
			self.last_change = time.asctime()
		
		def print_changes(self):
			print 'Changelog for Slate object:'
			for k, v in self.history.items():
				print '%s\t %s' %(k, v)

		def __getstate__(self):
			# Deliberately do not return self.value or self.last_change.
			# We want to have a "blank slate" when we unpickle
			return self.history

		def __setstate__(self, state):
			# make self.history = state and last_change and value undefined
			self.history = state
			self.value, self.last_change = None, None

###参考

中文链接: [Python魔术方法指南](http://pycoders-weekly-chinese.readthedocs.org/en/latest/issue6/a-guide-to-pythons-magic-methods.html)

英文链接: [A Guide to Python's Magic Methods](http://www.rafekettler.com/magicmethods.html)

