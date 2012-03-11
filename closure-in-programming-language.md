## 程序设计语言中的闭包(closure)概念
wikipedia上的定义为:

In computer science, a closure(also lexical closure, function closure, function value or functional value) is a function together with a referencing environment for the non-local variables of that function.

也就是说，闭包 = 函数 + 环境，什么是环境(a referencing environment)呢？环境就是被函数引用的变量的名称与值。这些变量只能被闭包中的函数引用，修改，并且闭包的函数每次调用结束，这些变量的生命期并没有结束，它们是和闭包的生命一样长的。只有当不再有闭包的函数引用它们，它们才和闭包一起消失。这和语言的垃圾收集机制相关，所以一般支持闭包的语言都支持垃圾收集。

那么闭包的实现形式是什么样的呢？如下是一段Python代码：

    def counter():
		x = 0
		def increment(y):
			nonlocal x
			x += y
			print(x)
		return increment
    counter1_increment = counter()
	counter2_increment = counter()
    
    counter1_increment(1)	#prints 1
	counter1_increment(7)   #prints 8
	counter2_increment(1)   #prints 1
	counter1_increment(1)   #prints 9

代码中定义了一个函数counter,函数中有一个局部变量x，以及一个嵌套定义的函数increment，increment函数访问了x, 对increment来说，x是一个外部变量。这样，counter函数调用返回的就是一个闭包，闭包中包含一个对increment函数的引用以及变量x。

`counter1_increment = counter()`

`counter2_increment = counter()`

这两句代码分别创建了一个闭包，这两个闭包是相互独立的，不相关的，其中的变量x对它们来说都是私有的，不会相互影响。
