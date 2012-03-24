## Python学习
### Generator

*Generator* is a simple and powerful tool for creating iterators. They are written like regular functions but use the *yield* statement whenever they want to return data. Each time *next()* is called, the genertor resumes where it left-off (it remembers all the data values and which statement was last executed). An example shows that generators can be trivially easy to create:

	def reverse(data):
		for index in range(len(data)-1,-1,-1):
			yield data[index]

	>>>for char in reverse('golf'):
	...		print char
	...	
	f
	l
	o
	g

Anything that can be done with generators can also be done with class based iterators as described in the previous section. What makes generators so compact is that the *\_\_iter\_\_* and *next()* methods are created automatically.

Another key feature is that the local variables and execution state are automatically saved between calls. This made the function easier to write and much more clear than an approach using instance variables like self.index and self.data.

### zip()

	zip(seq1, [, seq2 [...]]) -> [(seq1[0], seq2[0] ...), (...)])

Return a list of tuples, where each tuple contains the i-th element from each of the argument sequences. The returned list is truncated in length to the length of the shortest argument sequence.

	>>> xvec = [10, 20, 30]
	>>> yvec = [7, 5, 3]
	>>> sum(x*y for x, y in zip(xvec, yvec))
	260

### subprocess

The *subprocess* module allows you to spawn new process, connect to their input/output/error pipes, and obtain their return code. This module intends to replace several other, older modules and functions, such as:

	os.system
	os.spawn*
	os.popen*
	popen2.*
	commands.*

### Django

- 把数据存取逻辑，业务逻辑和表现逻辑组合在一起的概念有时被称为软件架构的Model-View-Controller(MVC)模式。在这个模式中，Model代表数据存取层，View代表的是系统中选择显示什么和怎么显示的部分，Controller指的是系统中根据用户输入并视需要访问模型，以决定使用哪个视图的哪个部分。

Django中M，V和C各自的含义:

> M, 数据存取部分，由django数据库层处理。

> V, 选择显示哪些数据以及怎样显示的部分，由视图和模板处理。

> C, 根据用户输入委派视图的部分，由Django框架根据URLconf设置，对给定URL调用适当的Python函数。

由于C由框架自行处理，而Django里更关注的是模型(Model)，模板(Template)和视图(Views)，Django也被称为MTV框架

---

- 在Django中Project和app的区别？

> 一个project包含很多个Django app以及对它们的配置。

> 技术上，project的作用是提供配置文件，比方说哪里定义数据库连接信息，安装的app列表，TEMPLATE_DIRS，等等。

> 一个app是一套Django功能的集合，通常包括模型和视图，按python的包结构的方式存在。例如，Django本身内建有一些app，例如注释系统和自动管理系统。app的一个关键点是它们是很容易移植到其他project和被多个project复用。

django系统对app有一个约定：如果你使用了Django的数据库层(模型)，你必须创建一个Django app。模型必须存放在apps中。

---

- Django模型相关的命令

> python manage.py validate --- 检查模型的语法和逻辑是否正确

> python manage.py sqlall appName --- 生成CREATE TABLE语句，但并没有在数据库中真正创建数据表，只是把SQL语句段打印出来，这样你可以看到Django究竟会做些什么。

> python manage.py syncdb --- 同步你的模型到数据库的一个简单方法。它会根据INSTALLED_APPS里设置的app来检查数据库，如果表不存在，它就会创建它。要注意的是syncdb并不能将模型的修改或删除同步到数据库；如果你修改或删除了一个模型，并想把它提交到数据库，syncdb并不会做出任何处理。

---

- Django的Admin是如何工作的？

当服务启动时，Django从url.py引动URLconf，然后执行admin.autodiscover()语句。这个函数遍历INSTALLED_APPS配置，并且寻找相关的admin.py文件。如果在指定的app目录下找到admin.py，它就执行其中的代码。例如:

在books应用程序目录下的admin.py文件中，每次调用admin.site.register()都将那个模块注册到管理工具中。管理工具只为那些明确注册了的模块显示一个编辑/修改的界面。

应用程序*django.contrib.auth*包含自身的admin.py，所以Users和Groups能在管理工具中自动显示。其它的django.contrib应用程序，如django.contrib.redirects，其它从网上下载的第三方Django应用程序一样，都会自行添加到管理工具。

综上所述，管理工具其实就是一个Django应用程序，包含自己的模型，模板，视图和URLpatterns。你要像添加自己的视图一样，把它添加到URLconf里面。

---

- 类字典对象

*request.GET和request.POST是类字典对象*，意思是它们的行为像Python里标准的字典对象，但在技术底层上它们不是标准字典对象。比如说，request.GET和request.POST都有get(),keys()和values()方法，你可以用for key in request.GET获取所有的键。

那到底有什么区别呢？因为request.GET和request.POST拥有一些普通的字典对象所没有的方法。

---

- POST和GET

POST数据来自HTML中的\<form\>标签提交的，而GET数据可能来自\<form\>提交也可能是URL中的查询字符串(the query string)

---

- Django带有一个form库，称为django.forms，这个库可以处理HTML表单显示，数据校验，数据清理和表单错误重现。

> 表单框架最主要的用法是，为每一个将要处理的HTML的*\<Form\>*定义一个Form类。

> forms框架把每一个字段的显示逻辑分离到一组部件(widget)中。每一个字段类型都拥有一个默认的部件。

### virtualenv

virtualenv是一个建立虚拟Python环境的工具。

安装好后，直接运行

	virtualenv	env_dir
	source env_dir/bin/activate

就完成环境目录的建立，并激活了该环境。最新的1.7版本默认是不使用系统原有的包(等于--no-site-packages)。激活环境后，安装的包都会限制在这个环境里(应该就是env_dir目录里)，执行deactive会离开这个环境。

移除环境直接删除该目录应该就可以了。

---

*virtualenv* is a tool to create isolated Python environments.

The basic problem being addressed is one of dependencies and versions, and indirectly permissions. Imagine you have an application that needs version 1 of LibFoo, but another application requires version 2. How can you use both these applications? If you install everything into /usr/lib/python2.7/site-packages (or whatever your platform\'s standard location is), it\'s easy to end up in a situation where you unintentionally upgrade an application that shoudn\'t be upgraded.

Or more generally, what if you want to install an application and leave it be? If an application works, any change in its libraries or the version of those libraries can break the application.

Also, what if you can\'t install packages into the global site-packages directory? For instance, on a shared host.

In all these cases, virtualenv can help you. It creates an environment that has its own installation directories, that doesn\'t share libraries with other virtualenv environment(and optionally doesn\'t access the globally installed libraries either).

The basic usage is:

	$python virtualenv.py ENV

If you install it you can also just do virtual ENV

This creates ENV/lib/pythonX.X/site-packages, where any libraries you install will go. It also creates ENV/bin/python, which is a Python interpreter that uses this environment. Anytime you use that interpreter(including when a script has *#!/path/to/ENV/bin/python in it*)the libraries in that environment will be used.

---

- The *--system-site-packages* Option

If you build with virtualenv --system-site-packages ENV, your virtual environment will inherit packages from /usr/lib/python2.7/site-packages(or wherever your global site-packages directory is).

This can be used if you have control over the global site-packages directory, and you want to depend on the packages there. If you want to isolation from the global system, do not use this flag.

### Built-in Functions

**abs(x)**: Return the absolute value of a number. The argument may be a plain or long integer or a floating point number. If the argument is a complex number, its magnitude is returned.

**bin(x)**: Convert an integer number to a binary string.

**bool([x])**: Convert a value to a Boolean, using the standard truth testing procedure. If x is false or omitted, this returns False; otherwise it returns True.

**callable(object)**: Return True if the object argument appears callable, False if not. If this returns true, it is still possible that a call fails, but if it is false, calling object will never succeed.

**chr(i)**: Return a string of one character whose ASCII code is the integer i.

**cmp(x, y)**: Compare the two objects x and y and return an integer according the ourcome. The return value is negative if x<y, zero if x == y and strictly positive if x > y.

**delattr(object, name)**: This is a relative of *setattr()*. The arguments are an object and a string. The string must be the name of one of the object\'s attributes. The function deletes the named attribute, provided the object allows it.

**dict([arg])**: Create a new data dictionary, optionally with items taken from arg.

**divmod(a, b)**: Take two(non complex)numbers as arguments and return a pair of numbers consisting of their quotient and remainder when using long division.

**enumerate(sequence[, start=0])**: Return an enumerate object. sequence must be a sequence, an iterator, or some other object which supports iteration.The next() method of the iterator returned by enumerate() returns a tuple containing a count (from *start* which defaults to 0) and the values obtained from iterating over sequence:

	>>>seasons = ['Spring','Summer','Fall','Winter']
	>>>list(enumerate(seasons))
	[(0,'Spring'),(1,'Summer'),(2,'Fall'),(3,'Winter')]
	>>>list(enumerate(seasons, start=1))
	[(1,'Spring'),(2,'Summer'),(3,'Fall'),(4,'Winter')]

Equivalent to:

	def enumerate(sequence, start=0):
		n = start
		for elem in sequence:
			yield n, elem
			n += 1

**eval(expression[,globals[,locals]])**: The arguments are a string and optional globals and locals. If provided, globals must be a dictionary. If provided, locals can be any mapping object.

**filter(function, iterable)**: Construct a list from those elements of iterable for which function returns true. iterable may be either a sequence, a container which supports iteration, or an iterator. If iterable is a string or a tuple, the result also has that type; otherwise it is always a list.

**getattr(object,name[,default])**: Return the value of the named attribute of object. name must be a string. If the string is the name of one of the object\'s attributes, the result is the value of that attribute. If the named attribute does not exist, *default* is returned if provided, otherwise AttributeError is raised.

**hasattr(object name)**: The arguments are an object and a string. The result is True if the string is the name of one of the object\'s attributes, False if not.(This is implemented by calling getattr(object,name) and seeing whether it raises an exception or not).

**hex(x)**: Convert an integer number (of any size) to a hexadecimal(十六进制的) string. The result is a valid Python expression.

**isinstance(object, classinfo)**: Return true if the object argument is an instance of the classinfo argument, or of a (direct, indirect or virtual) subclass thereof. Also return true if classinfo is a type object (new-style class) and object is an object of that type or of a (direct, indirect or virtual) subclass thereof.

**issubclass(class, classinfo)**

**map(function, iterable, ...)**: Apply function to every item of iterable and return a list of the results.

**next(iterator[,default])**: Retrieve the next item from the iterator by calling its next() method. If *default* is given, it is returned if the iterator is exhausted, otherwise StopIteration is raised.

**oct(x)**: Convert an integer number(of any size) to an octal string.

**pow(x,y[,z])**: Return x to the power y, if z is present, return x to the power y, modulo z(computed more efficiently than pow(x,y)% z). The two-argument form pow(x,y) is equivalent to using the power operator: x\*\*y.

**reduce(function, iterable[, initializer])**: Apply *function* of two arguments cumulatively to the items of iterable, from left to right, so as to reduce the iterable to a single value.

**repr(object)**: Return a string containing a printable representation of an object. A class can control what this function returns for its instances by defining a \_\_repr\_\_() method.

**reversed(seq)**: Return a reverse iterator. seq must be an object which has a \_\_reversed\_\_ method.

**slice([start], stop[, step])**

**sorted(iterable[, cmp[, key[,reverse]]])**

**type(object)**: Return the type of an object.

**vars([object])**: Return the \_\_dict\_\_ attribute for a module, class, instance, or any other object with a \_\_dict\_\_ attribute.

**xrange([start],stop[,step])**

### 不定参数

- 以一个\*开始的参数，代表一个任意长的元组


	def mul(\*arg):
		print arg
	
	mul(1,2,3,4,5,6,7,'hello','xiayf')   #输出元组(1,2,3,4,5,6,7,'hello','xiayf')

- 一个以\**开始的参数，代表一个字典


	def mul2(\**arg):
		print arg
	
	mul2(a=11,b=444,c=888)          #输出一个字典{'a':11,'c':888,'b':444}

两种参数前者可以直接写实参，后者则写成'名=值'的形式

### Code Style

- Idioms

Idiomatic(地道的) Python code is often referred to as being **Pythonic**

- Upacking

If you know the length of a list or tuple, you can assign names to its elements with unpacking:

	for index, item in enumerate(some_list):
		# do something with index and item

You can use this to swap variables, as well:

	a, b = b, a

- Create an ignored variable

If you need to assign something (for instance, in Upacking)but not need that variable, use **\_**:

	filename = 'foobar.txt'
	basename, _, ext = filename.rpartition()

> "_" is commonly used as an alias for the *gettext()* function. If your application uses (or may someday use) *gettext*, you may want to avoid using _ for ignored variables, as you may accidentally shadow *gettext()*.

- Create a length-N list of the same thing

Use the Python list * operator:

	four_nones = [None] * 4

- Create a length-N list of lists

Because lists are mutable, the * operator will create a list of N references to the same list, which is not likely what you want. Instead, use a list comprehension:

	four_lists = [[] for _ in xrange(4)]

- Check if variable equals a constant

You don't need to explicitly compare a value to True, or None, or 0, you can just add it to the if statement:

**Bad**:

	if attr == True:
		print 'True!'
	
	if attr == None:
		print 'attr is None'
	
**Good**:

	# Just check the value
	if attr:
		print 'True!'
	# or check for the opposite
	if not attr:
		print 'attr is None'

- Access a Dictionary Element

Don't use the *has_key* function. Instead use *x in d* syntax, or pass a default argument to *get*:

**Bad**:

	d = {'hello':'world'}
	if d.has_key('hello'):
		print d['hello']
	else:
		print 'default_value'

**Good**:

	d = {'hello':'world'}

	print d.get('hello', 'default_value')  # prints 'world'
	print d.get('thingy', 'default_value')	# prints 'default_value'

	# or
	if 'hello' in d:
		print d['hello']

- Short ways to manipulate Lists

List comprehensions provide a powerful, concise way to work with lists. Also, the **map** and **filter** functions can perform operations on lists using a different concise syntax.

**Bad**

	# Filter elements less than 5
	a = [3 ,4 ,5]
	b = []
	for i in a:
		if i > 4:
			b.append(i)

**Good**

	b = [i for i in a if i > 4]
	# or
	b = filter(lambda : x > 4, a)

**Bad**

	# Add three to all list members
	a = [3, 4, 5]
	count = 0
	for i in a:
		a[count] = i + 3
		count = count + 1

**Good**

	a = [3, 4, 5]
	a = [i + 3 for i in a]
	# Or
	a = map(lambda i: i + 3, a)

Use **enumerate** to keep a count of your place in the list

	for index, item in enumerate(a):
		print index + ', ' + item

- Read From a File

Use the **with open** syntax to read from files. This will automatically close files for you.

**Bad**

	f = open('file.txt')
	a = f.read()
	print a
	f.close()

**Good**

	with open('file.txt') as f:
		for line in f:
			print line

- Returning Multiple Values from a Function

Python supports returning multiple values from a function as a comma-separated list, so you don't have to create an object or dictionary and pack multiple values in before you return

**Bad**

	def math_func(a):
		return {'square': a ** 2, 'cube': a ** 3}

	d = math_func(3)
	s = d['square']
	c = d['cube']

**Good**

	def math_func(a):
		return a ** 2, a ** 3

	square, cube = math_func(3)

### Parallel Processing

- Run in thread Python decorator

Here is a little decorator that will run a function in a thread:

	import threading

	def run_in_thread(fn):
		def run(*k, **kw):
			t = threading.Thread(target=fn, args=k, kwargs=kw)
			t.start()
		return run

It can be used for lazy updating analytics, cleanups or search index. An example:

	class Search:

		@run_in_thread
		def add_item(self, ...):
			...

- Run multiple functions in threads.

This is especially useful if you are scripting something and you need parallel processing.

How to use it:

	import os
	from functools import partial

	def expensive_operation(sleep_time):
		print 'Sleeping for %s seconds...' % sleep_time
		os.popen('sleep %s'%(sleep_time))
		print 'Slept for %s seconds'%sleep_time

	run_in_threads(
			partial(expensive_operation, 10),
			partial(expensive_operation, 5),
			partial(expensive_operation, 15)
	)

How it's implemented:

	import threading

	def run_in_threads(*functions):
		threads = []

		for fn in functions:
			thread = threading.Thread(target=fn)
			thread.start()
			threads.append(thread)

		for thread in threads:
			thread.join()

- Set timeout for a shell command in Python

I wanted to run a shell command in python without knowing if the shell command is going to exit within reasonable time.

	def timeout_command(command, timeout):
		'''call shell-command and either return its output or kill it
		   if it doesn't normally exit within timeout seconds and return None
		'''
		import subprocess, datetime, os, time, signal

		cmd = command.split(' ')
		start = datetime.datetime.now()
		process = subprocess.Popen(cmd, stdout=subprocess.PIPE, stderr=subprocess.PIPE)

		while process.poll() is None:
			time.sleep(0.1)
			now = datetime.datetime.now()
			if (now - start).seconds > timeout:
				os.kill(process.pid, signal.SIGKILL)
				os.waitpid(-1, os.WNOHANG)
				return None
		return process.stdout.read()

The process can be killed when it has run for too long(the *os.waitpid* waits for the kill to end and avoids defunct-processes) and furthermore the Popen'ed process' printed is caught and returned if it doesn't timeout.

### Processing XML in Python

Python has quite a few tools available in the standard library to handle XML.

**xml.dom.\*** modules: implement the W3C DOM API. If you're used to working with the DOM API or have some requirement to do so, this package can help you. Note that there are several modules in the xml.dom package, representing different tradeoffs between performance and expressivity.

**xml.sax.\*** modules: implement the SAX API, which trades convenience for speed and memory consumption. SAX is an event-based API meant to parse huge documents "on the fly" without loading them wholly into memory.

**xml.parser.expat**: a direct, low level API to the C-based expat parser. The expat interface is based on event callbacks, similarly to SAX. But unlike SAX, the interface is non-standard and specific to the expat library.

Finally, there's **xml.etree.ElementTree** (from now on, **ET** in short). It provides a lightweight Pythonic API, backed by an effcient C implementation, for parsing and creating XML. Compared to DOM, ET is much faster and has a more pleasant API to work with. Compared to SAX, there is ET.iterparse which also provides "on the fly" parsing without loading the whole document into memory. The performance is on par with SAX, but the API is higher level and much more convenient to use.

My recommendation is to always use ET for XML processing in Python, unless you have very specific needs that may call for the other solutions.

- ElementTree - one API, two implementations

ElementTree is an API for manipulating XML, and it has two implementations in the Python standard library. One is a pure Python implementation in *xml.etree.ElementTree*, and the other is an accelerated C implementation in *xml.etree.cElementTree*. It's important to remember to always use the C implementation, since it is much, much faster and consumes significantly less memory. If your code can run on platforms that might not have the \_elementtree extension module available, the import incantation you need is:

	try:
		import xml.etree.cElementTree as ET
	except ImportError:
		import xml.etree.ElementTree as ET

This is a common pratice in Python to choose from several implementations of the same API. Although chances are that you'll be able to get away with just importing the first module, your code *may* end up running on some strange platform where this will fail, so you better prepare for the possibility. Note that starting with Python 3.3, this will no longer be needed, since the ElementTree module will look for the C accelerator itself and fall back on the Python implementation if that's not available. So it will be sufficient to just import xml.etree.ElementTree.

- Parsing XML into a tree

XML is an inherently hierarchical data format, and the most natural way to represent it is with a tree. ET has two objects for this purpose - *ElementTree* represents the whole XML document as a tree, and *Element* represents a single node in the tree. Interactions with the whole document (reading, writing, finding interesting elements) are usually done on the *ElementTree* level. Interactions with a single XML element and its sub-elements is done on the *Element* level.

### PEP 343 -- The "with" Statement

This PEP adds a new statement "with" to the Python language to make it possible to factor out standard uses of try/finally statements.

**Specification**:
	
	with EXPR as VAR:
		BLOCK

here, 'with' and 'as' are new keywords; EXPR is an arbitrary expression (but not an expression-list) and VAR is a single assignment target. It can *not* be a comma-separated sequence of variables, but it *can* be a *parenthesized* comma-separated sequence of variables.(This restriction makes a future extension possible of the syntax to have multiple comma-separated resources, each with its own optional as-clause.)

The "as VAR" part is optional.

The translation of the above statement is:

	mgr = (EXPR)
	exit = type(mgr).__exit__	# Not calling it yet
	value = type(mgr).__enter__(mgr)
	exc = True
	try:
		try:
			VAR = value		# Only if "as VAR" is present
			BLOCK
		except:
			# The exceptional case is handled here
			exc = False
			if not exit(mgr, *sys.exc_info()):
				raise
			# The exception is swallowed if exit() returns true
	finally:
		# The normal and non-local-goto cases are handled here
		if exc:
			exit(mgr, None, None, None)

Here, the lowercase variable (mgr, exit, value, exc) are internal variables and not accessible to the user; they will most likely be implemented as special registers or stack positions. 

**contextlib --- Utilities for with-statement contexts**

This module provides utilities for common tasks involving the **with** statement.

Functions provided:

contextlib.**contextmanager**(func)

> This function is a *decorator* that can be used to define a factory function for *with* statement context managers, without needing to create a class or separate \_\_enter\_\_() and \_\_exit\_\_ methods.

A simple example (this is not recommended as a real way of generating HTML!):

	from contextlib import contextmanager

	@contextmanager
	def tag(name):
		print "<%s>" % name
		yield
		print "</%s>" % name

	>>> with tag("h1"):
	...		print "foo"
	...
	<h1>
	foo
	</h1>

contextlib.**nested**(mgr1[,mgr2[,...]])

> Combine multiple context managers into a single nested context managers/

contextlib.**closing**(thing)

> Return a context manager that closes *thing* upon completion of the block. This is basically equivalent to:

	from contextlib import contextmanager

	@contextmanager
	def closing(thing):
		try:
			yield thing
		finally:
			thing.close()

And lets you write code like this:

	from contextlib import closing
	import urllib

	with closing(urllib.urlopen('http://www.python.org')) as page:
		for line in page:
			print line

without needing to explicitly close *page*. Even if an error occurs, *page.close()* will be called when the **with** block is exited.

> **原理上还不是很懂，具体内容参考:1.http://docs.python.org/library/contextlib.html; 2.http://www.python.org/dev/peps/pep-0343/**
