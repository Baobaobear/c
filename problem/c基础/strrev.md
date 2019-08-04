# 字符串反向

输入一个字符串并回车，反向输出它。例如输入abc，输出cba

<details>
<summary>写法1 入门版本</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(void)
{
	char str[100];
	int len;
	scanf("%100s", str);
	for (int i = 0; str[i]; ++i)
	{
		len = i;
	}
	for (int i = len; i >= 0; --i)
	{
		printf("%c", str[i]);
	}
	return 0;
}
```
</details>

<details>
<summary>写法2 数组逆序实现</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(void)
{
	char str[100];
	int len;
	scanf("%100s", str);
	for (int i = 0; str[i]; ++i)
	{
		len = i;
	}
	for (int i = 0; i < (len + 1) / 2; ++i)
	{
		char c = str[i];
		str[i] = str[len - i];
		str[len - i] = c;
	}
	printf("%s", str);
	return 0;
}
```
</details>

<details>
<summary>写法3 使用标准库函数实现</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <string.h>

int main(void)
{
	char str[100];
	scanf("%100s", str);
	strrev(str);
	printf("%s", str);
	return 0;
}
```
</details>

<details>
<summary>写法4 递归</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

void rev_out()
{
	char c = getchar();
	if (c != '\n')
	{
		rev_out();
		putchar(c);
	}
}

int main(void)
{
	rev_out();
	return 0;
}
```
</details>
