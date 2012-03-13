## Python学习

- *Generator* is a simple and powerful tool for creating iterators. They are written like regular functions but use the *yield* statement whenever they want to return data. Each time *next()* is called, the genertor resumes where it left-off (it remembers all the data values and which statement was last executed). An example shows that generators can be trivially easy to create:

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
