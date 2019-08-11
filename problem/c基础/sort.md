# 排序

对一个数组内容从小到大排序

<details>
<summary>写法1 冒泡排序</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(void)
{
	int arr[] = {9, 5, 2, 7, 1, 3, 8, 6, 4};
	int n = sizeof(arr) / sizeof(*arr);

	for (int j = 0; j < n; ++j)
	{
		for (int i = 1; i < n - j; ++i)
		{
			if (arr[i - 1] > arr[i])
			{
				int t = arr[i - 1];
				arr[i - 1] = arr[i];
				arr[i] = t;
			}
		}
	}

	for (int j = 0; j < n; ++j)
	{
		printf("%d ", arr[j]);
	}

	return 0;
}
```
</details>

<details>
<summary>写法2 选择排序</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(void)
{
	int arr[] = {9, 5, 2, 7, 1, 3, 8, 6, 4};
	int n = sizeof(arr) / sizeof(*arr);

	for (int j = 0; j < n; ++j)
	{
		int min_index = j;
		for (int i = j + 1; i < n; ++i)
		{
			if (arr[min_index] > arr[i])
			{
				min_index = i;
			}
		}
		int t = arr[j];
		arr[j] = arr[min_index];
		arr[min_index] = t;
	}
	
	for (int j = 0; j < n; ++j)
	{
		printf("%d ", arr[j]);
	}

	return 0;
}
```
</details>

<details>
<summary>写法3 函数封装冒泡排序</summary>

冒泡排序的函数封装写法，带上优化
```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

void bubble_sort(int arr[], int n)
{
	for (int j = 0; j < n; ++j)
	{
		int flag = 1;
		for (int i = 1; i < n - j; ++i)
		{
			if (arr[i - 1] > arr[i])
			{
				int t = arr[i - 1];
				arr[i - 1] = arr[i];
				arr[i] = t;
				flag = 0;
			}
		}
		if (flag)
			break;
	}
}

int main(void)
{
	int arr[] = {9, 5, 2, 7, 1, 3, 8, 6, 4};
	int n = sizeof(arr) / sizeof(*arr);

	bubble_sort(arr, n);

	for (int j = 0; j < n; ++j)
	{
		printf("%d ", arr[j]);
	}

	return 0;
}
```
</details>

<details>
<summary>写法4 插入排序</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

void insert_sort(int arr[], int n)
{
	for (int i = 0; i < n; i++)
	{
		int val = arr[i], j = i;
		for (; j >= 1 && val < arr[j - 1]; --j)
		{
			arr[j] = arr[j - 1];
		}
		arr[j] = val;
	}
}

int main(void)
{
	int arr[] = {9, 5, 2, 7, 1, 3, 8, 6, 4};
	int n = sizeof(arr) / sizeof(*arr);

	insert_sort(arr, n);

	for (int j = 0; j < n; ++j)
	{
		printf("%d ", arr[j]);
	}

	return 0;
}
```
</details>

<details>
<summary>写法5 希尔排序</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

void shell_sort(int arr[], int n)
{
	int step = 1;
	while (step < n / 2)
	{
		step = step * 3 + 1;
	}
	for ( ; step >= 1; step /= 3)
	{
		for (int i = step; i < n; i++)
		{
			int val = arr[i], j = i;
			for (; j >= step && val < arr[j - step]; j -= step)
			{
				arr[j] = arr[j - step];
			}
			arr[j] = val;
		}
	}
}

int main(void)
{
	int arr[] = {9, 5, 2, 7, 1, 3, 8, 6, 4};
	int n = sizeof(arr) / sizeof(*arr);

	shell_sort(arr, n);

	for (int j = 0; j < n; ++j)
	{
		printf("%d ", arr[j]);
	}

	return 0;
}
```
</details>

<details>
<summary>写法6 使用库函数</summary>

使用的是stdlib中的qsort函数，此函数使用的是快速排序
```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>

int cmp(const void *a, const void *b)
{
	return (*(int *)a - *(int *)b);
}

int main(void)
{
	int arr[] = {9, 5, 2, 7, 1, 3, 8, 6, 4};
	int n = sizeof(arr) / sizeof(*arr);

	qsort(arr, n, sizeof(*arr), cmp);

	for (int j = 0; j < n; ++j)
	{
		printf("%d ", arr[j]);
	}

	return 0;
}
```
</details>

<details>
<summary>写法7 快速排序</summary>

本篇由于代码量多，只写出排序函数本身

快速排序对元素的遍历处理主要有以下三种

<details>
<summary>实现 1</summary>

固定选取第一个元素，用交替覆盖方式实现交换效果  

```c
int partition(int arr[], int high)
{
	int low = 0;
	int pivot = arr[low];
	while (low < high)
	{
		while (low < high && pivot < arr[high] )
			high--;
		arr[low] = arr[high];
		while (low < high && !(pivot < arr[low]) )
			low++;
		arr[high] = arr[low];
	}
	arr[low] = pivot;
	return low;
}

void quick_sort(int arr[], int n)
{
	if (1 < n)
	{
		int loc = partition(arr, n - 1);
		quick_sort(arr, loc++);
		quick_sort(arr + loc, n - loc);
	}
}
```
</details>

<details>
<summary>实现 2</summary>

固定选取第一个元素，对左右采用元素交换的方式  

```c
void swap(int *x, int *y)
{
	int t = *x;
	*x = *y;
	*y = t;
}

void quick_sort_recursive(int * begin, int * end)
{
	if (begin + 1 < end)
	{
		int *l = begin, *r = end - 1;
		int pivot = *begin;
		while (l < r)
		{
			while (l < r && *r >= pivot)
				--r;
			while (l < r && *l <= pivot)
				++l;
			if (l < r)
				swap(l, r);
		}
		swap(l, begin);
		quick_sort_recursive(begin, l);
		quick_sort_recursive(l + 1, end);
	}
}

void quick_sort(int arr[], int len)
{
	quick_sort_recursive(arr, arr + len);
}
```
</details>

<details>
<summary>实现 3</summary>

固定选取最末元素，对小于pivot的元素放最左边  
是以上三种写法里速度最慢的写法  

```c
void swap(int *x, int *y)
{
	int t = *x;
	*x = *y;
	*y = t;
}

int partition(int *arr, int front, int end)
{
	int pivot = arr[end];
	int i = front - 1;
	for (int j = front; j < end; j++)
	{
		if (arr[j] < pivot)
		{
			i++;
			swap(&arr[i], &arr[j]);
		}
	}
	i++;
	swap(&arr[i], &arr[end]);
	return i;
}

void quick_sort(int *arr, int end)
{
	if (1 < end)
	{
		int pivot = partition(arr, 0, end - 1);
		quick_sort(arr, pivot++);
		quick_sort(arr + pivot, end - pivot);
	}
}
```
</details>

以上为三种快速排序的基本实现，也是网上常见的版本

以上三种都有相同的缺陷，对升序或降序的序列特别慢，甚至会发生错误，不应在应用场景中使用

在应用场景下使用快排，必须使用其优化版本

</details>

<details>
<summary>写法8 快速排序优化</summary>

以下优化代码基于前面的实现2修改而来

<details>
<summary>优化 1</summary>

近似随机选取比较元素法
```c
void swap(int *x, int *y)
{
	int t = *x;
	*x = *y;
	*y = t;
}

void quick_sort_recursive_rand(int * begin, int * end)
{
	if (begin + 1 < end)
	{
		static int s_rnd = 0xffffff;
		swap(begin + (++s_rnd) % (end - begin), begin);
		int *l = begin, *r = end - 1;
		int pivot = *begin;
		while (l < r)
		{
			while (l < r && *r >= pivot)
				--r;
			while (l < r && *l <= pivot)
				++l;
			if (l < r)
				swap(l, r);
		}
		swap(l, begin);
		quick_sort_recursive_rand(begin, l);
		quick_sort_recursive_rand(l + 1, end);
	}
}

void quick_sort_rand(int arr[], int len)
{
	quick_sort_recursive_rand(arr, arr + len);
}
```
</details>

<details>
<summary>优化 2</summary>

三数取中，在开头、中间、结尾三个数里找出中间值作为分割

```c
void swap(int *x, int *y)
{
	int t = *x;
	*x = *y;
	*y = t;
}

void mid_pivot(int *l, int *r, int *mid)
{
	if (*mid > *r)
		swap(mid, r);
	if (*l > *mid)
		swap(l, mid);
	if (*mid > *r)
		swap(mid, r);
}

int* quick_sort_partition_mid1(int * begin, int * end)
{
	mid_pivot(begin, end - 1, begin + (end - begin) / 2);
	swap(begin + (end - begin) / 2, begin);
	int *l = begin, *r = end - 1;
	int pivot = *begin;
	while (l < r)
	{
		while (l < r && *r >= pivot)
			--r;
		while (l < r && *l <= pivot)
			++l;
		if (l < r)
			swap(l, r);
	}
	swap(l, begin);
	return l;
}

void quick_sort_recursive_mid1(int * begin, int * end)
{
	if (begin + 1 < end)
	{
		int *p = quick_sort_partition_mid1(begin, end);
		quick_sort_recursive_mid1(begin, p);
		quick_sort_recursive_mid1(p + 1, end);
	}
}

void quick_sort_mid1(int arr[], int len)
{
	quick_sort_recursive_mid1(arr, arr + len);
}
```
</details>

<details>
<summary>优化 3</summary>

基于前一个代码，过滤相等的数据

在大量相同数据下减少陷入最坏情况

```c
void swap(int *x, int *y)
{
	int t = *x;
	*x = *y;
	*y = t;
}

void mid_pivot(int *l, int *r, int *mid)
{
	if (*mid > *r)
		swap(mid, r);
	if (*l > *mid)
		swap(l, mid);
	if (*mid > *r)
		swap(mid, r);
}

void quick_sort_recursive_mid2(int * begin, int * end)
{
	if (begin + 1 < end)
	{
		mid_pivot(begin, end - 1, begin + (end - begin) / 2);
		swap(begin + (end - begin) / 2, begin);
		int *l = begin, *r = end - 1;
		int pivot = *begin;
		while (l < r)
		{
			while (l < r && *r >= pivot)
				--r;
			while (l < r && *l <= pivot)
				++l;
			if (l < r)
				swap(l, r);
		}
		swap(l, begin);
		quick_sort_recursive_mid2(begin, l);
		while (++l < end && *l == pivot);
		quick_sort_recursive_mid2(l, end);
	}
}

void quick_sort_mid2(int arr[], int len)
{
	quick_sort_recursive_mid2(arr, arr + len);
}
```

</details>

<details>
<summary>优化 4</summary>

基于前一个代码，分割数组时严格分割为小于pivot和大于等于pivot的两段

这样在大量相同数据下也几乎不会陷入最坏情况

```c
void swap(int *x, int *y)
{
	int t = *x;
	*x = *y;
	*y = t;
}

void mid_pivot(int *l, int *r, int *mid)
{
	if (*mid > *r)
		swap(mid, r);
	if (*l > *mid)
		swap(l, mid);
	if (*mid > *r)
		swap(mid, r);
}

void quick_sort_recursive_mid3(int * begin, int * end)
{
	if (begin + 1 < end)
	{
		mid_pivot(begin, end - 1, begin + (end - begin) / 2);
		swap(begin + (end - begin) / 2, begin);
		int *l = begin, *r = end - 1;
		int pivot = *begin;

		while (l < r)
		{
			while (l < r && !(*r < pivot))
				r--;
			while (*l < pivot)
				l++;
			if (l < r)
				swap(l, r);
		}
		quick_sort_recursive_mid3(begin, l);
		while (l < end && *l == pivot)
			++l;
		quick_sort_recursive_mid3(l, end);
	}
}

void quick_sort_mid3(int arr[], int n)
{
	quick_sort_recursive_mid3(arr, arr + n);
}
```

</details>

<details>
<summary>优化 5</summary>

基于前一个代码，记录递归深度，在深度过大的时候转而使用堆排序

这样完全避免在非常特殊的数据下运行时间过长或堆栈溢出

从而让这个实现成为实用级别，保证最坏情况下的算法复杂度是O(nlogn)

这个算法叫做内省排序（Introsort）

代码需要引用堆排序，请自行添加

```c
int* intro_sort_partition(int * begin, int * end)
{
	mid_pivot(begin, end - 1, begin + (end - begin) / 2);
	swap(begin + (end - begin) / 2, begin);
	int pivot = *begin;
	int *l = begin, *r = end - 1;

	while (l < r)
	{
		while (l < r && !(*r < pivot))
			r--;
		while (*l < pivot)
			l++;
		if (l < r)
			swap(l, r);
	}
	return l;
}

void intro_sort_recursive(int * begin, int * end, int depth)
{
	if (begin + 1 >= end)
		return;

	if (depth <= 0)
	{
		heap_sort(begin, end - begin);
		return;
	}

	int *p = intro_sort_partition(begin, end);

	intro_sort_recursive(begin, p, depth - 1);
	intro_sort_recursive(p, end, depth - 1);
}

void intro_sort1(int arr[], int n)
{
	int depth = log(n) * 2;
	intro_sort_recursive(arr, arr + n, depth);
}
```

</details>

快排的基本优化介绍到这里，关于快排仍有很多细节可优化，但这里不展开讨论了

</details>

<details>
<summary>写法9 堆排序</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

void swap(int *x, int *y)
{
	int t = *x;
	*x = *y;
	*y = t;
}

void max_heapify(int arr[], int index, int length)
{
	int child, temp = arr[index];
	for (; 2 * index + 1 < length; index = child)
	{
		child = 2 * index + 1;
		if (child < length - 1 && arr[child + 1] > arr[child])
			++child;
		if (temp < arr[child])
		{
			arr[index] = arr[child];
		}
		else
			break;
	}
	arr[index] = temp;
}

void heap_sort(int arr[], int length)
{
	int i;
	for (i = length / 2 - 1; i >= 0; --i)
		max_heapify(arr, i, length);
	for (i = length - 1; i > 0; --i)
	{
		swap(&arr[0], &arr[i]);
		max_heapify(arr, 0, i);
	}
}

int main(void)
{
	int arr[] = {9, 5, 2, 7, 1, 3, 8, 6, 4};
	int n = sizeof(arr) / sizeof(*arr);

	heap_sort(arr, n);

	for (int j = 0; j < n; ++j)
	{
		printf("%d ", arr[j]);
	}

	return 0;
}
```
</details>

<details>
<summary>写法10 归并排序</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>

void merge_sort_recursive(int arr[], int reg[], int start, int end)
{
	if (start >= end)
		return;
	int len = end - start, mid = (len >> 1) + start;
	int start1 = start, end1 = mid;
	int start2 = mid + 1, end2 = end;
	merge_sort_recursive(arr, reg, start1, end1);
	merge_sort_recursive(arr, reg, start2, end2);
	int k = start;
	while (start1 <= end1 && start2 <= end2)
		reg[k++] = arr[start1] < arr[start2] ? arr[start1++] : arr[start2++];
	while (start1 <= end1)
		reg[k++] = arr[start1++];
	while (start2 <= end2)
		reg[k++] = arr[start2++];
	for (k = start; k <= end; k++)
		arr[k] = reg[k];
}

void merge_sort(int arr[], int len)
{
	int* reg = (int*)malloc(len * sizeof(*arr));
	merge_sort_recursive(arr, reg, 0, len - 1);
	free(reg);
}

int main(void)
{
	int arr[] = {9, 5, 2, 7, 1, 3, 8, 6, 4};
	int n = sizeof(arr) / sizeof(*arr);

	merge_sort(arr, n);

	for (int j = 0; j < n; ++j)
	{
		printf("%d ", arr[j]);
	}

	return 0;
}
```
</details>

<details>
<summary>写法11 基数排序</summary>

要注意的是此算法不是基于比较的排序

而且此实现只能对非负整数排序
```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>

const int radix_sort_base = 16;

int maxbit(int data[], int n)
{
	int maxData = data[0];
	for (int i = 1; i < n; ++i)
	{
		if (maxData < data[i])
			maxData = data[i];
	}
	int d = 1;
	int p = radix_sort_base;
	while (maxData >= p)
	{
		maxData /= radix_sort_base;
		++d;
	}
	return d;
}

void radix_sort(int data[], int n)
{
	int d = maxbit(data, n);
	int *tmp = (int *)malloc(n * sizeof(int));
	int *count = (int *)malloc(radix_sort_base * sizeof(int));
	int i, j, k;
	int radix = 1;
	for (i = 0; i < d; i++) //进行d次排序
	{
		for (j = 0; j < radix_sort_base; j++)
			count[j] = 0;
		for (j = 0; j < n; j++)
		{
			k = (data[j] / radix) % radix_sort_base; //统计每个桶中的记录数
			count[k]++;
		}
		for (j = 1; j < radix_sort_base; j++)
			count[j] = count[j - 1] + count[j];
		for (j = n - 1; j >= 0; j--) //将所有桶中记录依次收集到tmp中
		{
			k = (data[j] / radix) % radix_sort_base;
			tmp[--count[k]] = data[j];
		}
		for (j = 0; j < n; j++) //将临时数组的内容复制到data中
			data[j] = tmp[j];
		radix = radix * radix_sort_base;
	}
	free(tmp);
	free(count);
}

int main(void)
{
	int arr[] = {9, 5, 2, 7, 1, 3, 8, 6, 4};
	int n = sizeof(arr) / sizeof(*arr);

	radix_sort(arr, n);

	for (int j = 0; j < n; ++j)
	{
		printf("%d ", arr[j]);
	}

	return 0;
}
```
</details>

# 排序算法运行时间比较

以下比较使用 VS2015 x64 Release 环境编译并执行  
处理器为I7 4790K  
运行时间的绝对值在不同机器上结果区别可能会很大  
统计的时间也存在误差，以下数据仅供参考，且会随着文档变动而更新

表格中的时间单位是**毫秒(ms)**，数值越小即越快  
**ERROR**表示运行时发生栈溢出。  
**TIMEOUT**表示运行时间超过25秒

名字对照|中文名
-|-
bubble	|冒泡排序（写法3）
select	|选择排序（封装写法2）
insert	|插入排序（写法4）
shell	|希尔排序（写法5）
qsort	|C标准的qsort函数
std::s	|C++ STL的std::sort函数
q_simp1	|快速排序（写法7，实现1）
q_simp2	|快速排序（写法7，实现2）
q_simp3	|快速排序（写法7，实现3）
q_rand	|快速排序优化1
q_mid1	|快速排序优化2
q_mid2	|快速排序优化3
q_mid3	|快速排序优化4
intro1	|快速排序优化5
intro2	|introsort优化版本2，小于16个元素时使用插入排序
intro3	|introsort优化版本3
heap	|堆排序（写法9）
merge	|归并排序（写法10）
radix	|基数排序（写法11）

### 测试数据生成方式

1. 1, 全等数据
2. n, 有序数据
3. 1000000000 - n, 逆序数据
4. n, 生成后shuffle, 无序无重复数据
5. n % (len * 90 / 100), 生成后shuffle, 无序10%数据重复
6. n % (len * 50 / 100), 生成后shuffle, 无序50%数据重复
7. n % (len / 100), 生成后shuffle, 无序99%数据重复
8. n % 1024, 生成后shuffle, 无序大量重复数据
9. n % 16, 生成后shuffle, 无序极大量重复数据
10. n % 1000, 部分有序大量重复数据

## 排序算法运行时间比较1 （数据规模20K个int）

 -|  bubble|    heap|  insert|  q_mid1|  q_rand| q_simp1| q_simp2| q_simp3|  select|   shell|
-:|  -----:|  -----:|  -----:|  -----:|  -----:|  -----:|  -----:|  -----:|  -----:|  -----:|
 1|       0|       0|       0|      80|      51|      71|      51|     101|     103|       0|
 2|       0|       1|       0|       0|       1|      51|      53|     140|     103|       0|
 3|     253|       1|      81|       1|       0|      61|      51|     122|     100|       1|
 4|     434|       1|      42|       1|       1|       1|       1|       1|     105|       1|
 5|     434|       1|      40|       1|       2|       1|       1|       1|     105|       1|
 6|     433|       1|      41|       1|       0|       1|       1|       1|     103|       1|
 7|     435|       1|      41|       1|       1|       1|       1|       1|     102|       1|
 8|     438|       1|      41|       1|       1|       1|       0|       0|     103|       1|
 9|     427|       1|      40|       4|       3|       5|       3|       7|     102|       1|
10|     177|       1|      39|       0|       1|       1|       0|       8|     102|       0|

## 排序算法运行时间比较2 （数据规模50K个int）

 -|  bubble|    heap|  insert|  q_mid1|  q_rand| q_simp1| q_simp2| q_simp3|  select|   shell|
-:|  -----:|  -----:|  -----:|  -----:|  -----:|  -----:|  -----:|  -----:|  -----:|  -----:|
 1|       0|       0|       0|     492|     323|   ERROR|     317|     635|     632|       1|
 2|       0|       2|       0|       1|       1|     317|     320|   ERROR|     633|       0|
 3|    1580|       2|     518|       0|       0|   ERROR|   ERROR|   ERROR|     630|       1|
 4|    2942|       3|     254|       3|       3|       3|       2|       2|     637|       3|
 5|    2931|       3|     257|       3|       3|       2|       3|       2|     638|       4|
 6|    2939|       3|     254|       3|       3|       3|       2|       2|     642|       4|
 7|    2937|       3|     256|       4|       2|       2|       2|       2|     636|       3|
 8|    2924|       3|     255|       3|       3|       2|       3|       2|     636|       3|
 9|    2905|       3|     240|      21|      19|      29|      18|      41|     637|       2|
10|    1116|       4|     247|       3|       2|       2|       1|      20|     643|       1|

## 排序算法运行时间比较3 （数据规模200K个int）

去掉了前一表中最慢的 bubble, select, insert

 -|    heap|   merge|  q_mid1|  q_mid2|  q_mid3|  q_rand| q_simp1| q_simp2| q_simp3|   shell|
-:|  -----:|  -----:|  -----:|  -----:|  -----:|  -----:|  -----:|  -----:|  -----:|  -----:|
 1|       1|       5|    7668|       1|       0|    5111|   ERROR|    5087|   10110|       2|
 2|       8|       5|       2|       2|       2|       4|    5112|    5078|   ERROR|       2|
 3|       8|       6|       3|       3|       3|       4|   ERROR|   ERROR|   ERROR|       3|
 4|      14|      17|      12|      12|      12|      13|      10|      12|      11|      18|
 5|      16|      16|      12|      12|      12|      13|      11|      11|      11|      19|
 6|      16|      16|      11|      12|      12|      14|      11|      11|      11|      18|
 7|      15|      15|      12|      10|       7|      14|      13|      11|      16|      16|
 8|      14|      15|      13|      11|       6|      14|      17|      12|      19|      15|
 9|       9|      10|     326|     191|       3|     275|     471|     279|     636|       7|
10|      13|       7|      11|       9|       3|      13|      14|      11|      82|       6|

## 排序算法运行时间比较4 （数据规模3M个int）

去掉了前一表中最慢出错最多的 q_simp1, q_simp3

 -|    heap|  intro1|   merge|  q_mid1|  q_mid2|  q_mid3|  q_rand| q_simp2|   qsort|   shell|
-:|  -----:|  -----:|  -----:|  -----:|  -----:|  -----:|  -----:|  -----:|  -----:|  -----:|
 1|       7|      54|     112| TIMEOUT|       4|       3| TIMEOUT| TIMEOUT|      16|      56|
 2|     159|      48|     106|      35|      37|      37|      70| TIMEOUT|     161|      49|
 3|     156|      69|     106|      35|      37|      57|      69|   ERROR|     162|      58|
 4|     397|     227|     288|     212|     214|     220|     220|     202|     392|     394|
 5|     401|     238|     285|     210|     212|     219|     218|     204|     380|     379|
 6|     393|     248|     285|     211|     211|     212|     220|     196|     383|     394|
 7|     381|     181|     258|     218|     185|     159|     223|     209|     279|     365|
 8|     322|     145|     218|    1206|     728|      91|    1104|    1056|     186|     259|
 9|     159|      85|     175|   ERROR|   ERROR|      37|   ERROR|   ERROR|      79|     118|
10|     252|     286|     111|    1240|     722|      54|    1086|    1083|     178|     140|

## 排序算法运行时间比较5 （数据规模10M个int）

去掉了前一表中最慢的 q_rand, q_simp2, q_mid1

 -|    heap|  intro1|  intro2|  intro3|   merge|  q_mid2|  q_mid3|   qsort|   shell|  std::s|
-:|  -----:|  -----:|  -----:|  -----:|  -----:|  -----:|  -----:|  -----:|  -----:|  -----:|
 1|      29|     199|      10|     138|     398|       9|      12|      51|     171|       6|
 2|     520|     161|      83|      81|     364|     117|     120|     578|     165|     127|
 3|     561|     324|      87|      82|     376|     128|     208|     587|     205|     143|
 4|    2156|     812|     659|     634|    1060|     756|     786|    1371|    1640|     800|
 5|    2139|     835|     657|     636|    1049|     759|     782|    1381|    1624|     804|
 6|    2139|     902|     656|     628|    1047|     752|     759|    1359|    1623|     796|
 7|    2094|     657|     477|     516|     958|     673|     507|     987|    1566|     594|
 8|    1389|     471|     283|     369|     776|    6991|     302|     602|     970|     343|
 9|     552|     287|     126|     247|     620|   ERROR|     129|     268|     440|     133|
10|    1248|    1394|     162|     262|     408|    7454|     168|     587|     357|     212|

## 排序算法运行时间比较6 （数据规模30M个int）

去掉了前一表中最慢的 qsort, q_mid2, heap, shell

 -|  intro1|  intro2|  intro3|   merge|  q_mid3|   radix|  std::s|
-:|  -----:|  -----:|  -----:|  -----:|  -----:|  -----:|  -----:|
 1|     582|      30|     429|    1263|      29|     300|      16|
 2|     500|     252|     246|    1227|     380|    1854|     387|
 3|    1188|     259|     257|    1278|     697|    1934|     455|
 4|    2600|    2087|    2019|    3464|    2529|    1672|    2605|
 5|    2707|    2135|    2068|    3484|    2549|    1664|    2596|
 6|    2887|    2114|    2040|    3426|    2443|    1466|    2568|
 7|    2098|    1565|    1665|    3141|    1650|    1195|    1952|
 8|    1433|     854|    1143|    2482|     897|     742|    1027|
 9|    1263|     383|     782|    1986|     406|     300|     396|
10|    5691|     506|     779|    1427|     499|     836|     520|

下表是 mingw32 5.1.0 版本使用 -O2 编译运行的结果，已测试与 8.1.0 差异微小，但mingw的运行时间有点诡异

 -|  intro1|  intro2|  intro3|   merge|  q_mid3|   radix|  std::s|
-:|  -----:|  -----:|  -----:|  -----:|  -----:|  -----:|  -----:|
 1|     705|      32|     367|    1342|      30|     291|     409|
 2|     517|     774|     258|    1303|     403|    1781|     458|
 3|    1549|     761|     256|    1323|     788|    1834|     363|
 4|    3911|    2241|    1617|    2836|    6706|    1710|    3752|
 5|    2461|    2498|    1767|    2840|    2577|    1770|    1839|
 6|    3088|    4304|    2598|    2780|    2492|    1605|    1989|
 7|    2512|    1919|    1625|    2827|    1754|    1150|    1675|
 8|    1968|    1054|    1145|    2585|    1026|     747|    1178|
 9|    1617|     477|     656|    2101|     426|     296|     745|
10|    6627|     711|     719|    1427|     619|     813|     774|

下表是 clang 8.0.1 版本使用 -O3 编译运行的结果

 -|  intro1|  intro2|  intro3|   merge|  q_mid3|   radix|  std::s|
-:|  -----:|  -----:|  -----:|  -----:|  -----:|  -----:|  -----:|
 1|     572|      29|     397|    1838|      29|     288|      15|
 2|     470|     305|     248|    1832|     375|    1789|     339|
 3|    1162|     318|     260|    1884|     703|    1849|     401|
 4|    2573|    2094|    1992|    2834|    2579|    1595|    2597|
 5|    2623|    2101|    1994|    2827|    2560|    1563|    2589|
 6|    2880|    2136|    2018|    2831|    2486|    1413|    2572|
 7|    2119|    1560|    1619|    2827|    1658|    1142|    1960|
 8|    1481|     853|    1112|    2829|     901|     719|    1028|
 9|    1328|     383|     712|    2846|     408|     290|     398|
10|    5705|     608|     725|    2395|     519|     811|     541|


## 基于比较的排序算法实现本次测试综合表现排行（排除radix）：
1. intro3
2. intro2
3. std::s
4. intro1
5. q_mid3
6. merge
7. qsort
8. shell
9. heap
10. q_mid2
11. q_mid1
12. q_rand
13. q_simp2
14. q_simp1
15. q_simp3
16. insert
17. select
18. bubble

测试数据中的4，5，8是现实应用中大多数遇到的场景，尤其是4和5，所以这几行测试数据结果的权重更高
