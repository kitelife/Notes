## C语言学习

### 联合

当用联合来将各种不同大小的数据类型结合在一起时，字节顺序问题就变得很重要了。例如，假设我们写了一个过程，它会以两个4字节的unsigned的位的形式，创建一个8字节的double:

	double bit2double(unsigned word0, unsigned word1)
	{
		union {
			double d;
			unsigned u[2];
		} temp;
	
		temp.u[0] = word0;
		temp.u[1] = word1;
		return temp.d;
	}

在像IA32这样的小端法(little-endian)机器上，参数word0会是d的低位四个字节，而word1会是高位四个字节。在大端法(big-endian)机器上，这两个参数的角色刚好相反。

### 指针

- 函数指针

> 函数指针声明的语法对程序员新手来说是特别难以理解的。对于这样一个声明:
>
>	void (*f) (int *);
>
> 要从里(从"f"开始)往外读。因此，我们看到像"(*f)"声明的那样，f是一个指针。像"(*f)(int *)"声明的那样，它是一个指针，指向一个以一个int *作为参数的函数。最后，我们看到，它是一个指向一个以int *为参数并返回void的函数的指针。
> *f两边的括号是必须的，因为否则声明：
> void *f(int *);
> 就要读成
> (void *) f(int *);
> 也就是，它会被解释成一个函数原型，声明一个函数f,它以一个int *作为参数并返回一个void *。

### 存储器的越界引用和缓冲区溢出

C对于数组引用不进行任何边界检查，而且局部变量和状态信息(如寄存器值和返回指针)都存放在栈中。这两种情况结合到一起就能导致严重的程序错误，一个对越界的数组元素的写操作破坏了存储在栈中的状态信息。然后，当程序使用这个被破坏的状态，试图重新加载寄存器或执行ret指令时，就会出现很严重的错误。

### 可变参数函数

> <stdarg.h>头文件定义了一些宏，当函数参数未知时去获取函数的参数。
> 
> 变量：typedef va_list
> 宏：
> va_start()
> va_arg()
> va_end()
>
> 变量和定义
> va_list类型通过stdarg宏定义来访问一个函数的参数列表，参数列表的末尾会用省略号省略。
> 声明：void va_start(va_list ap, last_arg);
> 用va_arg和va_end宏初始化参数ap，last_arg是传给函数的固定函数的最后一个，省略号之前的那个参数，注意va_start必须在使用va_arg和va_end之前调用
>
> 声明：type va_arg(va_list ap, type);
> 用type类型扩展到参数表的下个参数
> 注意ap必须用va_start初始化，如果没有下一个参数，结果会是undefined
>
> 声明：void va_end(va_list ap);
> 允许一个有参数表(使用va_start宏)的函数返回，如果返回之前没有调用va_end，结果会是undefined。参数变量列表可能不再使用(在没调用va_start的情况下调用va_end)

要创建一个可变参数函数，必须把省略号(...)放到参数列表后面。函数内部必须定义一个va_list变量，然后使用宏va_start，va_arg和va_end来读取。例如：

	#include <stdarg.h>

	double average(int count, ...)
	{
		va_list ap;
		int j;
		double tot = 0;
		va_start(ap, count);
		for(j=0; j<count; j++)
		{
			tot += va_arg(ap, double);
		}
		va_end(ap); //释放va_list
		return tot/count;
	}
