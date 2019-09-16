# 阶乘的和

输入n，计算 1! + 2! + ... + n!


<details>
<summary>写法1 入门版本 1</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(void)
{
	int sum = 0, factor = 1;
	int n;
	scanf("%d", &n);
	for (int i = 1; i <= n; ++i)
	{
		factor *= i;
		sum += factor;
	}
	printf("%d", sum);
	return 0;
}
```
</details>

<details>
<summary>写法2 入门版本 2</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(void)
{
	int factor[20] = {1};
	int n, sum = 0;
	scanf("%d", &n);
	for (int i = 1; i <= n; ++i)
	{
		factor[i] = factor[i-1] * i;
	}
	for (int i = 1; i <= n; ++i)
	{
		sum += factor[i];
	}
	printf("%d", sum);
	return 0;
}
```
</details>

<details>
<summary>写法3 递归实现1</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int sum(int n)
{
	if (n < 2) return n;
	return (sum(n - 1) - sum(n - 2)) * n + sum(n - 1);
}

int main(void)
{
	int n;
	scanf("%d", &n);
	printf("%d", sum(n));
	return 0;
}
```
</details>

<details>
<summary>写法4 递归实现2</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int sum(int n, int s)
{
	if (s >= n) return n;
	return (sum(n, s + 1) + 1) * s;
}

int main(void)
{
	int n;
	scanf("%d", &n);
	printf("%d", sum(n, 1));
	return 0;
}
```
</details>
