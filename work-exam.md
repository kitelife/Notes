2012-网易-校园招聘-C++开发工程师
==================================

Fibonacci number
------------------

F(n)的值是多少？

![Fabonacci](https://lh6.googleusercontent.com/-0n60YMZUq5c/UHZZgx84uQI/AAAAAAAABHQ/hbn9epSpIaI/s174/NumberedEquation6.gif)

**常规算法**：根据Fabonacci的定义，递归求值。时间复杂度O(2^n)

	def fibobacci(n):
		return n>=2 and fibonacci(n-2) + fibonacci(n-1) or n

**迭代**：利用循环取代递归。时间复杂度O(n)

	def fibonacci(n):
		a, b = 0, 1
		for i in range(n):
			a, b = b, a+b
		return a

*迭代算法的变种* ：利用python的generator，原理一样，只是应用场合不一样。

	def fibonacci():
		a, b = 0, 1
		while 1:
			yield a
			a, b = b, a+b
	
	f = fibonacci()
	n = 100
	for i in range(n+1):
		print(next(f)

**利用矩阵运算来加速的算法**：（还没懂）算法复杂度O(logn)

	import numpy

	fibonacci_matrix = numpy.matrix([1,1],[1,0], dtype=numpy.ndarray)
	def fibonacci(n):
		return fibonacci_matrix**(n-1)[0, 0]

**数学方法:使用Fibonacci序列的通用公式**：

	from math import sqrt

	def fibonacci(n):
		return int(((1+sqrt(5))/2)**n/sqrt(5))

虽然数学方法在4种方法中速度最快，但当n越大，其计算精度偏差越大。
