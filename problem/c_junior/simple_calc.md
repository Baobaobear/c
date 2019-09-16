# 数字反向

输入第一个数、一个运算符、第二个数，计算结果。例如输入 1 + 2 ，输出结果3

运算符包括 + - * /

<details>
<summary>写法1 入门版本 1</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(void)
{
	double a, b;
	char op;
	scanf("%lf %c %lf", &a, &op, &b);
	if (op == '+')
	{
		printf("%g\n", a + b);
	}
	else if (op == '-')
	{
		printf("%g\n", a - b);
	}
	else if (op == '*')
	{
		printf("%g\n", a * b);
	}
	else if (op == '/')
	{
		if (b != 0)
		{
			printf("%g\n", a / b);
		}
		else
		{
			printf("illegal\n");
		}
	}
	else
	{
		printf("illegal\n");
	}
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
	double a, b;
	char op;
	scanf("%lf %c %lf", &a, &op, &b);
	switch (op)
	{
	case '+':
		printf("%g\n", a + b);
		break;
	case '-':
		printf("%g\n", a - b);
		break;
	case '*':
		printf("%g\n", a * b);
		break;
	case '/':
		if (b != 0)
		{
			printf("%g\n", a / b);
		}
		else
		{
			printf("illegal\n");
		}
		break;
	default:
		printf("illegal\n");
	}
	return 0;
}
```
</details>
