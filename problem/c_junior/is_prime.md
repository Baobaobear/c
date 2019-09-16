# 质数判断

输入一个正整数，如果这个数是质数，输出yes，否则输出no

<details>
<summary>写法1 入门版本</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(void)
{
	int i, n;
	scanf("%d", &n);
	for (i = 2; i < n; ++i)
	{
		if (n % i == 0)
			break;
	}
	if (i == n)
	{
		printf("%s", "yes");
	}
	else
	{
		printf("%s", "no");
	}
	return 0;
}
```
</details>

<details>
<summary>写法2 平方根优化</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <math.h>

int main(void)
{
	int i, n, flag = 1;
	int sqr;
	scanf("%d", &n);
	sqr = (int)(sqrt(n) + 0.5);
	for (i = 2; i <= sqr; ++i)
	{
		if (n % i == 0)
		{
			flag = 0;
			break;
		}
	}
	if (flag)
	{
		printf("%s", "yes");
	}
	else
	{
		printf("%s", "no");
	}
	return 0;
}
```
</details>

<details>
<summary>写法3 函数封装</summary>

此写法由写法2封装为函数实现，除此之外还能继续优化，请自行尝试

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <math.h>

int isPrime(int n)
{
	int sqr;
	sqr = (int)(sqrt(n) + 0.5);
	for (int i = 2; i <= sqr; ++i)
	{
		if (n % i == 0)
		{
			return 0;
		}
	}
	return 1;
}

int main(void)
{
	int n;
	scanf("%d", &n);
	if (isPrime(n))
	{
		printf("%s", "yes");
	}
	else
	{
		printf("%s", "no");
	}
	return 0;
}
```
</details>
