放入IDA中，是ELF文件，进行逆向分析
按下F12，找到关键函数
![[Pasted image 20251228143019.png]]
主函数
![[Pasted image 20251228143128.png]]
f函数，加密函数
![[Pasted image 20251228143147.png]]

追踪含有字符串的函数，发现此串字符串跟上面的加密函数有关系
![[Pasted image 20251228143045.png]]
进行分析后，发现是斐波那契生成数字来映射K里面的字符串来进行输出，然后当特定的n7出来时输出-。将优化好的c++代码写出来
```c++
#include<iostream>
#include<string>
#include<cstdint>
using namespace std;
int f(unsigned __int64 a1)
{
	char k[16] = { '0','1','2','a','b','9','c','3','4','7','8','d','5','6','e','f' };
	__int64 v2;
	unsigned __int64 i;
	__int64 v4;
	__int64 v5;
	v5 = 0;
	v4 = 1;
	for (i = 0; i < a1; ++i)
	{
		v2 = v4;
		v4 = ((uint8_t)v5 + (uint8_t)v4) & 0xF;
		v5 = v2;
	}
	return k[v5];
}
int main()
{
	__int64 v3;
	__int64 v4;
	uint32_t v6[4];
	uint8_t v7[12];
	int v8;
	char v9;
	int n7;
	unsigned __int64 v11;
	cout << "flag{";
	v11 = 1;
	for (n7 = 0; n7 <= 31; ++n7)
	{
		v9 = f(v11);
		cout << v9;
		if (n7 == 7 || n7 == 12 || n7 == 17 || n7 == 22)
		{
			cout << "-";
		}
		v11 *= 8LL;
		v11 += n7 + 64;
		v8 = 1;
	}
	cout << "}";
	return 0;
}
```
但是此时发现由于v11的增长，斐波那契数列输出时间呈指数级增长，所以需要对斐波那契数列生成算法进行优化，使用动态规划进行优化，代码如下

```c++
#include<iostream>
#include<string>
#include<cstdint>
#include<vector>
using namespace std;
pair<uint64_t, uint64_t>fib(uint64_t n) {
	if (n == 0)return { 0,1 };
	auto result = fib(n >> 1);
	uint64_t a = result.first;
	uint64_t b = result.second;
	uint64_t c = a * (2 * b - a);
	uint64_t d = a * a + b * b;
	if (n & 1)
		return { d,c + d };
	else
		return { c,d };
}
char f(unsigned __int64 a1) {
	uint32_t k[16] = { '0','1','2','a','b','9','c','3','4','7','8','d','5','6','e','f' };
	uint8_t fn = (uint8_t)fib(a1).first;
	return k[fn & 0xF];
};
int main()
{
	cout << "flag{";
	uint64_t v11 = 1;
	for (int n7 = 0; n7 <= 31; ++n7)
	{
		char v9 = f(v11);
		cout << v9;
		if (n7 == 7 || n7 == 12 || n7 == 17 || n7 == 22)
		{
			cout << "-";
		}
		v11 = v11*8+n7+ 64;
	}
	cout << "}";
	return 0;
}
```
得出flag
flag{10632674-1d219-09f29-14769-f60219a24}