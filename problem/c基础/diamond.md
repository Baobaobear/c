# 输出菱形

输出以下菱形
```
      *
     ***
    *****
   *******
  *********
 ***********
*************
 ***********
  *********
   *******
    *****
     ***
      *
```

<details>
<summary>写法1 入门版本1</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(void)
{
	int n = 6;
	for (int y = 0; y <= n; ++y)
	{
		for (int x = n; x > y; --x)
		{
			putchar(' ');
		}
		for (int x = 0; x < y * 2 + 1; ++x)
		{
			putchar('*');
		}
		putchar('\n');
	}
	for (int y = n - 1; y >= 0; --y)
	{
		for (int x = n; x > y; --x)
		{
			putchar(' ');
		}
		for (int x = 0; x < y * 2 + 1; ++x)
		{
			putchar('*');
		}
		putchar('\n');
	}
	return 0;
}
```
</details>

<details>
<summary>写法2 入门版本2</summary>

这是更常见更混乱的初学版本
```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(void)
{
	int i, j, n, line = 7;
	for (i = 0; i < line; i++)
	{
		for (n = 0; n < line - 1 - i; n++)
		{
			printf(" ");
		}
		for (j = 0; j < 2 * i + 1; j++)
		{
			printf("*");
		}
		printf("\n");
	}
	for (i = 0; i < line - 1; i++)
	{
		for (n = 0; n < i + 1; n++)
		{
			printf(" ");
		}
		for (j = 0; j < 2 * (line - 1 - i) - 1; j++)
		{
			printf("*");
		}
		printf("\n");
	}
	return 0;
}
```
</details>

<details>
<summary>写法3 坐标运算法</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(void)
{
	int n = 7;
	int i, j;
	for (i = 0; i < 2 * n - 1; i++)
	{
		for (j = 0; j < 2 * n - 1; j++)
		{
			if (i < n)
			{
				if (j >= n - i - 1 && j < n + i)
				{
					putchar('*');
				}
				else
				{
					putchar(' ');
				}
			}
			else
			{
				if (j >= i - n + 1 && j < 3 * n - i - 2)
				{
					putchar('*');
				}
				else
				{
					putchar(' ');
				}
			}
		}
		putchar('\n');
	}
	return 0;
}
```
</details>

<details>
<summary>写法4 巧取坐标系</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
	int n = 6;
	for (int j = -n; j <= n; ++j)
	{
		for (int i = -n; i <= n; ++i)
		{
			if (abs(i) + abs(j) <= n)
			{
				putchar('*');
			}
			else
			{
				putchar(' ');
			}
		}
		putchar('\n');
	}
	return 0;
}
```
</details>

<details>
<summary>写法5 巧用三元运算符</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
	int n = 6;
	for (int j = -n; j <= n; ++j)
	{
		for (int i = -n; i <= n; ++i)
		{
			putchar(abs(i) + abs(j) <= n ? '*' : ' ');
		}
		putchar('\n');
	}
	return 0;
}
```
</details>

<details>
<summary>写法6 巧用printf (1)</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
	const char* p = "      *****************";
	int n = 6;
	for (int j = -n; j <= n; ++j)
	{
		printf("%.*s\n",
			n * 2 + 1 - abs(j),
			p + n - abs(j));
	}
	return 0;
}
```
</details>

<details>
<summary>写法7 巧用printf (2)</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
	const char* p = "********************";
	int n = 6;
	for (int j = -n; j <= n; ++j)
	{
		printf("%*.*s\n",
			n * 2 - abs(j) + 1,
			n * 2 - abs(j) * 2 + 1,
			p);
	}
	return 0;
}
```
</details>

<details>
<summary>写法8 递归实现1</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

void spaces(int spaces_num)
{
	while (spaces_num --> 0)
		printf(" ");
}

void stars(int stars_num)
{
	while (stars_num --> 0)
		printf("*");
	printf("\n");
}

void print(int level, int stars_num)
{
	if (level == 0)
	{
		spaces(level);
		stars(stars_num * 2 + 1);
	}
	else
	{
		spaces(level);
		stars(stars_num * 2 + 1);
		print(level - 1, stars_num + 1);
		spaces(level);
		stars(stars_num * 2 + 1);
	}
}

int main(void)
{
	print(6, 0);
	return 0;
}
```
</details>

<details>
<summary>写法9 递归实现2</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

const char* p = "********************";
int n = 7;

int main(int a, char* v[])
{
	if (a < n)
	{
		printf("%*s%.*s\n", n - a, "", a * 2 - 1, p);
		main(a + 1, v);
		printf("%*s%.*s\n", n - a, "", a * 2 - 1, p);
	}
	else
	{
		printf("%.*s\n", a * 2 - 1, p);
	}
	return 0;
}
```
</details>
