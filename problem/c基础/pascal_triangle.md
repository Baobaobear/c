# Pascal三角

Pascal三角，又叫杨辉三角，即以下数字阵列

```
1
1 1
1 2 1
1 3 3 1
1 4 6 4 1
1 5 10 10 5 1
1 6 15 20 15 6 1
1 7 21 35 35 21 7 1
1 8 28 56 70 56 28 8 1
```

特点是下面的数字是它上一行相邻两数的和。输出这个数字阵列

<details>
<summary>写法1 入门版本</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(void)
{
	int arr[10][10] = {{1}};
	for (int j = 1; j < 9; ++j)
	{
		arr[j][0] = arr[j][j] = 1;
		for (int i = 1; i < j; ++i)
		{
			arr[j][i] = arr[j - 1][i - 1] + arr[j - 1][i];
		}
	}
	for (int j = 0; j < 9; ++j)
	{
		for (int i = 0; i <= j; ++i)
		{
			printf("%d ", arr[j][i]);
		}
		printf("\n");
	}
	return 0;
}
```
</details>

<details>
<summary>写法2 巧取坐标简化代码</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(void)
{
	int arr[10][10] = {{0, 1}};
	for (int j = 1; j < 9; ++j)
	{
		for (int i = 1; i <= j + 1; ++i)
		{
			arr[j][i] = arr[j - 1][i - 1] + arr[j - 1][i];
		}
	}
	for (int j = 0; j < 9; ++j)
	{
		for (int i = 1; i <= j + 1; ++i)
		{
			printf("%d ", arr[j][i]);
		}
		printf("\n");
	}
	return 0;
}

```
</details>

<details>
<summary>写法3 代码合并</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(void)
{
	int arr[10][10] = {{0, 1}};
	printf("1\n");
	for (int j = 1; j < 9; ++j)
	{
		for (int i = 1; i <= j + 1; ++i)
		{
			arr[j][i] = arr[j - 1][i - 1] + arr[j - 1][i];
			printf("%d ", arr[j][i]);
		}
		printf("\n");
	}
	return 0;
}

```
</details>

<details>
<summary>写法4 滚动数组</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(void)
{
	int arr[2][10] = {{0, 1}};
	printf("1\n");
	for (int j = 1; j < 9; ++j)
	{
		for (int i = 1; i <= j + 1; ++i)
		{
			arr[j & 1][i] = arr[(j - 1) & 1][i - 1] + arr[(j - 1) & 1][i];
			printf("%d ", arr[j & 1][i]);
		}
		printf("\n");
	}
	return 0;
}
```
</details>

<details>
<summary>写法5 单行数组</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(void)
{
	int arr[10] = {0, 1};
	printf("1\n");
	for (int j = 1; j < 9; ++j)
	{
		for (int i = j + 1; i > 0; --i)
		{
			arr[i] = arr[i - 1] + arr[i];
		}
		for (int i = 1; i <= j + 1; ++i)
		{
			printf("%d ", arr[i]);
		}
		printf("\n");
	}
	return 0;
}
```
</details>

<details>
<summary>写法6 数学公式法 1</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int Cmn(int m, int n)
{
	int s = 1;
	for (int i = 1; i < n; ++i)
	{
		s = s * (m - i) / i;
	}
	return s;
}

int main(void)
{
	for (int j = 1; j <= 9; ++j)
	{
		for (int i = 1; i <= j; ++i)
		{
			printf("%d ", Cmn(j, i));
		}
		printf("\n");
	}
	return 0;
}
```
</details>

<details>
<summary>写法7 数学公式法 2</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(void)
{
	for (int j = 1; j <= 9; ++j)
	{
		int s = 1;
		for (int i = 1; i <= j; ++i)
		{
			printf("%d ", s);
			s = s * (j - i) / i;
		}
		printf("\n");
	}
	return 0;
}
```
</details>
