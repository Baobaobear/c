# 数字反向

输入10个整数，去掉最高分和最低分后，计算剩下数值的平均分

<details>
<summary>写法1 入门版本 1</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(void)
{
	int score[10];
	int min_val, max_val, sum = 0;

	for (int i = 0; i < 10; ++i)
	{
		scanf("%d", &score[i]);
		sum += score[i];
	}
	min_val = score[0];
	max_val = score[0];
	for (int i = 1; i < 10; ++i)
	{
		if (score[i] < min_val)
			min_val = score[i];
		if (score[i] > max_val)
			max_val = score[i];
	}
	printf("avg = %g\n", (sum - max_val - min_val) / 8.0);
	return 0;
}
```
</details>

<details>
<summary>写法2 省略数组</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(void)
{
	int min_val, max_val, sum = 0, n;

	scanf("%d", &n);
	sum += n;
	min_val = n;
	max_val = n;

	for (int i = 1; i < 10; ++i)
	{
		scanf("%d", &n);
		sum += n;
		if (n < min_val)
			min_val = n;
		if (n > max_val)
			max_val = n;
	}

	printf("avg = %g\n", (sum - max_val - min_val) / 8.0);
	return 0;
}
```
</details>
