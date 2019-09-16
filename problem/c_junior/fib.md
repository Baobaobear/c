# 斐波那契数列

斐波那契数列就是 1, 1, 2, 3, 5, 8, 13, 21, 34, ...

特点是后一个数等于前两个数的和，请输出这个数列的前20个数

<details>
<summary>写法1 入门版本</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(void)
{
	int fib[20] = {1, 1};

	for (int i = 2; i < 20; ++i)
	{
		fib[i] = fib[i - 1] + fib[i - 2];
	}

	for (int i = 0; i < 20; ++i)
	{
		printf("%d ", fib[i]);
	}
	return 0;
}
```
</details>

<details>
<summary>写法2 省略一个循环</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(void)
{
	int fib[20] = {1, 1};

	printf("1 1");
	for (int i = 2; i < 20; ++i)
	{
		fib[i] = fib[i - 1] + fib[i - 2];
		printf(" %d", fib[i]);
	}

	return 0;
}
```
</details>

<details>
<summary>写法3 滚动数值 1</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(void)
{
	int m = 1, n = 1, s;

	printf("1 1");
	for (int i = 2; i < 20; ++i)
	{
		s = m + n;
		m = n;
		n = s;
		printf(" %d", s);
	}

	return 0;
}
```
</details>

<details>
<summary>写法4 滚动数值 2</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(void)
{
	int m = 1, n = 1;

	printf("1 1");
	for (int i = 2; i < 20; i += 2)
	{
		m += n;
		n += m;
		printf(" %d %d", m, n);
	}

	return 0;
}
```
</details>

<details>
<summary>写法5 递归</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int fib(int n)
{
	if (n <= 1)
		return 1;
	return fib(n - 1) + fib(n - 2);
}

int main(void)
{
	for (int i = 0; i < 20; ++i)
	{
		printf("%d ", fib(i));
	}

	return 0;
}
```
</details>
