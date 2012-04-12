C++语言学习
==========

其他
----

- endl是一个特殊值，称为**操纵符(manipulator)**，将它写入输出流时，具有输出换行的效果，并刷新与设备相关联的缓冲区。通过刷新缓冲区，用户可立即看到写入到流中的输出。

- *在写C++程序时，大部分出现空格符的地方可用换行符代替。这条规则的一个例外是字符串字面值中的空格符不能用换行符代替。另一个例外是空格符不允许出现在预处理指示中。*
- 读入未知数目的输入:

		#include <iostream>
		using namespace std;
		
		int main()
		{
			int sum = 0, value;
			// read till end-of-file, calculating a running total of all values read
			while(cin >> value)
				sum += value;

			cout << "Sum is: " << sum << endl;

			return 0;
		}