# 数字反向

输入一个数，反向它各个位得到一个新的数，再输出新的数值。例如输入01230，输出321  
也就是说需要正确处理前导0的情况

<details>
<summary>写法1 入门版本 1</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(void)
{
	int n, m = 0;
	scanf("%d", &n);
	for (; n > 0; n /= 10)
	{
		m = m * 10 + n % 10;
	}
	printf("%d", m);
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
	int n;
	scanf("%d", &n);
	while (n % 10 == 0)
	{
		n /= 10;
	}
	for (; n > 0; n /= 10)
	{
		printf("%d", n % 10);
	}
	return 0;
}
```
</details>

<details>
<summary>写法3 直接作为字符串实现</summary>

以下这种写法使用了scanf里的不常见写法过滤高位上的0

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <string.h>

int main(void)
{
	char str[100];
	int begin_pos;
	scanf("%*[0]%100s", str);
	strrev(str);
	for (begin_pos = 0; str[begin_pos] == '0'; ++begin_pos)
		;
	printf("%s", str + begin_pos);
	return 0;
}
```
</details>

<details>
<summary>写法4 函数封装</summary>

此法由写法1封装为函数实现

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int rev(int n)
{
	int m = 0;
	for (; n > 0; n /= 10)
		m = m * 10 + n % 10;
	return m;
}

int main(void)
{
	int n;
	scanf("%d", &n);
	printf("%d", rev(n));
	return 0;
}
```
</details>
