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
