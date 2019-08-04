# 分数等级

输入一个0到100的数  
如果是90分或以上，输出A  
80分或以上，输出B  
70分或以上，输出C  
60分或以上，输出D  
60以下输出E

<details>
<summary>写法1 入门版本</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(void)
{
	int n;
	scanf("%d", &n);
	if (n >= 90)
	{
		printf("%s", "A");
	}
	else if (n >= 80)
	{
		printf("%s", "B");
	}
	else if (n >= 70)
	{
		printf("%s", "C");
	}
	else if (n >= 60)
	{
		printf("%s", "D");
	}
	else
	{
		printf("%s", "E");
	}
	return 0;
}
```
</details>

<details>
<summary>写法2 使用switch</summary>

注意，此方法并不通用，仅作为一种思路提供
```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(void)
{
	int n;
	scanf("%d", &n);
	switch (n / 10)
	{
	case 10:
	case 9:
		printf("%s", "A");
		break;
	case 8:
		printf("%s", "B");
		break;
	case 7:
		printf("%s", "C");
		break;
	case 6:
		printf("%s", "D");
		break;
	default:
		printf("%s", "E");
	}

	return 0;
}
```
</details>

<details>
<summary>写法3 利用ASCII码取巧</summary>

注意，此方法并不通用，仅作为一种思路提供

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(void)
{
	int n;
	scanf("%d", &n);
	n = n / 10;
	if (n > 9)
	{
		n = 9;
	}
	else if (n < 5)
	{
		n = 5;
	}
	printf("%c", 'A' + 9 - n);

	return 0;
}
```
</details>

<details>
<summary>写法4 查表法</summary>

此方法较通用，推荐
```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <limits.h>

int main(void)
{
	int range[] = { 60,  70,  80,  90, INT_MAX};
	char rank[] = {'E', 'D', 'C', 'B', 'A'};
	int index, n;
	scanf("%d", &n);
	for (index = 0; n >= range[index]; ++index)
		;
	printf("%c", rank[index]);

	return 0;
}
```
</details>
