## 程序设计语言中的闭包(closure)概念
wikipedia上的定义为:

> In computer science, a closure(also lexical closure, function closure, function value or functional value) is a function together with a referencing environment for the non-local variables of that function.

也就是说，闭包 = 函数 + 环境，什么是环境(a referencing environment)呢？环境就是被函数引用的变量的名称与值。这些变量只能被闭包中的函数引用，修改，并且闭包的函数每次调用结束，这些变量的生命期并没有结束，它们是和闭包的生命一样长的。这和语言的垃圾收集机制相关，所以一般支持闭包的语言都支持垃圾收集。

那么闭包的在语言中的表现形式是什么样的呢？如下是一段Python代码：

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

这两句代码分别创建了一个闭包，这两个闭包是相互独立的，不相关的，其中的变量x对它们来说都是私有的，不会相互影响。从最后的四句代码可以看到这一点。并且从中可以看出，闭包中的环境(变量x)代表了闭包的状态。

在不同的语言中闭包在形式上略有差别，可能是函数嵌套(内层函数匿名或者非匿名)，面向对象的类，以及内部类等等。

但在实现原理应该是一致的。如果是ruby之父Matsumoto关于闭包的一段解释:

> Closure对象包含可以运行的代码，是可执行的，代码包含状态，执行范围。也就是说在Closure中你捕捉到运行环境，即局部变量。因此，你可以在一个Closure中引用局部变量，即使在函数调用已经返回之后，它的执行范围已经销毁，局部变量仍然作为一部分存在于Closure对象中，当没有任何对象引用它的时候，垃圾搜集器将处理它，局部变量将消失。

总结来说：一个闭包(更确切的说是函数闭包)由一个函数以及它的运行时环境(即它可以访问到的相对于它是外部变量(non-local)的局部变量的集合)，并且最重要的是:在整个闭包的生命周期内，环境只能被所属的闭包改变，别的函数或者闭包即看不到，更改变不了它们。这样说来，闭包也是一种封装的形式。

思考：强类型语言可以实现完全意义上的闭包么？比如C，C++...？

> 我认为强类型语言可以实现类似闭包的结构，但是无法做到真正封闭的闭包，为什么呢？因为，比如函数闭包，是通过闭包中的函数访问闭包的局部变量的，在强类型语言中，函数指针赋值时是需要声明函数的返回值类型的，这样就无法对闭包外部的代码完全隐藏内部的实现。而弱类型语言的函数返回值类型是自动推导的，不存在这个问题。(希望这样的理解是对的...:-))

参考以及进一步阅读--->[Wikipedia词条Closure](http://en.wikipedia.org/wiki/Closure_(computer_science),"Closure")
