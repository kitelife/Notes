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
