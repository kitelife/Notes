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

zip(seq1, [, seq2 [...]]) -> [(seq1[0], seq2[0] ...), (...)]

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

- 在Django中Project和app的区别？

> 一个project包含很多个Django app以及对它们的配置。

> 技术上，project的作用是提供配置文件，比方说哪里定义数据库连接信息，安装的app列表，TEMPLATE_DIRS，等等。

> 一个app是一套Django功能的集合，通常包括模型和视图，按python的包结构的方式存在。例如，Django本身内建有一些app，例如注释系统和自动管理系统。app的一个关键点是它们是很容易移植到其他project和被多个project复用。

django系统对app有一个约定：如果你使用了Django的数据库层(模型)，你必须创建一个Django app。模型必须存放在apps中。

- Django模型相关的命令

> python manage.py validate --- 检查模型的语法和逻辑是否正确

> python manage.py sqlall appName --- 生成CREATE TABLE语句，但并没有在数据库中真正创建数据表，只是把SQL语句段打印出来，这样你可以看到Django究竟会做些什么。

> python manage.py syncdb --- 同步你的模型到数据库的一个简单方法。它会根据INSTALLED_APPS里设置的app来检查数据库，如果表不存在，它就会创建它。要注意的是syncdb并不能将模型的修改或删除同步到数据库；如果你修改或删除了一个模型，并想把它提交到数据库，syncdb并不会做出任何处理。

- Django的Admin是如何工作的？

当服务启动时，Django从url.py引动URLconf，然后执行admin.autodiscover()语句。这个函数遍历INSTALLED_APPS配置，并且寻找相关的admin.py文件。如果在指定的app目录下找到admin.py，它就执行其中的代码。例如:

在books应用程序目录下的admin.py文件中，每次调用admin.site.register()都将那个模块注册到管理工具中。管理工具只为那些明确注册了的模块显示一个编辑/修改的界面。

应用程序*django.contrib.auth*包含自身的admin.py，所以Users和Groups能在管理工具中自动显示。其它的django.contrib应用程序，如django.contrib.redirects，其它从网上下载的第三方Django应用程序一样，都会自行添加到管理工具。

综上所述，管理工具其实就是一个Django应用程序，包含自己的模型，模板，视图和URLpatterns。你要像添加自己的视图一样，把它添加到URLconf里面。

- 类字典对象

*request.GET和request.POST是类字典对象*，意思是它们的行为像Python里标准的字典对象，但在技术底层上它们不是标准字典对象。比如说，request.GET和request.POST都有get(),keys()和values()方法，你可以用for key in request.GET获取所有的键。

那到底有什么区别呢？因为request.GET和request.POST拥有一些普通的字典对象所没有的方法。

- POST和GET

POST数据来自HTML中的\<form\>标签提交的，而GET数据可能来自<form>提交也可能是URL中的查询字符串(the query string)

- Django带有一个form库，称为django.forms，这个库可以处理HTML表单显示，数据校验，数据清理和表单错误重现。

> 表单框架最主要的用法是，为每一个将要处理的HTML的*\<Form\>*定义一个Form类。

> forms框架把每一个字段的显示逻辑分离到一组部件(widget)中。每一个字段类型都拥有一个默认的部件。


