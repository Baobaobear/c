# 排序

对一个数组内容从小到大排序。

为了代码通用性，我们对需要排序的类型使用别名 `typedef int sort_element_t;` 如需对其它类型排序，修改 `sort_element_t` 的定义即可。

另外只有前两个写法给出main函数，后面的代码对排序单纯写函数，于是不再写出main函数。

对于写法3及之后的代码，对要排序的数据类型均只通过小于号 `<` 来进行比较，所以类似 `!(a < b) && !(b < a)` 其实是比较这两数是否相等

<details>
<summary>写法1 冒泡排序</summary>

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
typedef int sort_element_t;

int main(void)
{
	sort_element_t arr[] = {9, 5, 2, 7, 1, 3, 8, 6, 4};
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
typedef int sort_element_t;

int main(void)
{
	sort_element_t arr[] = {9, 5, 2, 7, 1, 3, 8, 6, 4};
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
<summary>写法3 函数封装冒泡排序和选择排序</summary>

冒泡排序的函数封装写法，带上优化
```c
void sort_element_swap(sort_element_t *x, sort_element_t *y)
{
	sort_element_t t = *x;
	*x = *y;
	*y = t;
}

void _bubble_sort(sort_element_t * beg, sort_element_t * end)
{
	for (; beg < end; --end)
	{
		int flag = 1;
		for (sort_element_t * i = beg + 1; i < end; ++i)
		{
			if (*i < i[-1])
			{
				sort_element_swap(i - 1, i);
				flag = 0;
			}
		}
		if (flag)
			break;
	}
}

void bubble_sort(sort_element_t arr[], size_t n)
{
	_bubble_sort(arr, arr + n);
}
```

选择排序的函数封装写法
```c
void sort_element_swap(sort_element_t *x, sort_element_t *y)
{
	sort_element_t t = *x;
	*x = *y;
	*y = t;
}

void _select_sort(sort_element_t * beg, sort_element_t * end)
{
	for (; beg < end; ++beg)
	{
		sort_element_t* head = beg;
		for (sort_element_t* i = beg + 1; i < end; ++i)
		{
			if (*i < *head)
				head = i;
		}
		sort_element_swap(beg, head);
	}
}

void select_sort(sort_element_t arr[], size_t n)
{
	_select_sort(arr, arr + n);
}
```

</details>

<details>
<summary>写法4 插入排序</summary>

```c
void _insert_sort(sort_element_t* beg, sort_element_t* end)
{
	for (sort_element_t* i = beg; i < end; ++i)
	{
		sort_element_t val = *i;
		sort_element_t* j = i;
		for (; j > beg && val < j[-1]; --j)
		{
			*j = j[-1];
		}
		*j = val;
	}
}

void insert_sort(sort_element_t arr[], size_t n)
{
	_insert_sort(arr, arr + n);
}
```
</details>

<details>
<summary>写法5 希尔排序</summary>

```c
void _shell_sort(sort_element_t * beg, sort_element_t * end)
{
	size_t incre = 1;
	while ((int64_t)incre * 3 + 1 < end - beg)
		incre = incre * 3 + 1; // A003462, Pratt, 1971, Knuth, 1973
	for (; incre >= 1; incre /= 3)
	{
		sort_element_t * bound = beg + incre;
		for (sort_element_t * i = bound; i < end; ++i)
		{
			sort_element_t * p = i;
			sort_element_t val = *i;
			for (; p >= bound && val < *(p - incre); p -= incre)
				*p = *(p - incre);
			*p = val;
		}
	}
}

void shell_sort(sort_element_t arr[], size_t n)
{
	_shell_sort(arr, arr + n);
}
```
</details>

<details>
<summary>写法6 使用库函数</summary>

使用的是stdlib中的qsort函数，qsort函数是快速排序的一种优化实现

```c
int cmp(const void *a, const void *b)
{
	if (*(sort_element_t *)a < *(sort_element_t *)b)
	{
		return -1;
	}
	if (*(sort_element_t *)b < *(sort_element_t *)a)
	{
		return 1;
	}
	return 0;
}

void quick_sort_c(sort_element_t arr[], size_t n)
{
	qsort(arr, n, sizeof(*arr), cmp);
}
```
</details>

<details>
<summary>写法7 快速排序</summary>

本篇由于代码量多，只写出排序函数本身

快速排序对元素的遍历处理主要有以下3种

<details>
<summary>实现 1</summary>

用交替覆盖元素的方式实现

```c
sort_element_t* partition(sort_element_t * first, sort_element_t * last)
{
	sort_element_t pivot = *first;
	while (first < last)
	{
		while (first < last && pivot < *last)
			last--;
		*first = *last;
		while (first < last && !(pivot < *first))
			first++;
		*last = *first;
	}
	*first = pivot;
	return first;
}

void quick_sort_recursive(sort_element_t * beg, sort_element_t * end)
{
	if (end - beg > 1)
	{
		sort_element_t* p = partition(beg, end - 1);
		quick_sort_recursive(beg, p);
		quick_sort_recursive(p + 1, end);
	}
}

void quick_sort(sort_element_t arr[], size_t n)
{
	quick_sort_recursive(arr, arr + n);
}
```
</details>

<details>
<summary>实现 2</summary>

对左右采用元素交换的方式  

```c
void sort_element_swap(sort_element_t *x, sort_element_t *y)
{
	sort_element_t t = *x;
	*x = *y;
	*y = t;
}

sort_element_t* partition(sort_element_t * first, sort_element_t * last)
{
	sort_element_t * begin = first;
	sort_element_t pivot = *first;
	while (first < last)
	{
		while (first < last && !(*last < pivot))
			--last;
		while (first < last && !(pivot < *first))
			++first;
		if (first < last)
			sort_element_swap(first, last);
	}
	sort_element_swap(first, begin);
	return first;
}

void quick_sort_recursive(sort_element_t * beg, sort_element_t * end)
{
	if (end - beg > 1)
	{
		sort_element_t* p = partition(beg, end - 1);
		quick_sort_recursive(beg, p);
		quick_sort_recursive(p + 1, end);
	}
}

void quick_sort(sort_element_t arr[], size_t len)
{
	quick_sort_recursive(arr, arr + len);
}
```
</details>

<details>
<summary>实现 3</summary>

对小于等于pivot的元素交换到最左边  

```c
void sort_element_swap(sort_element_t *x, sort_element_t *y)
{
	sort_element_t t = *x;
	*x = *y;
	*y = t;
}

sort_element_t * partition(sort_element_t * beg, sort_element_t * end)
{
	sort_element_t pivot = *beg;
	sort_element_t * p = beg;
	for (sort_element_t * i = beg + 1; i < end; i++)
	{
		if (!(pivot < *i))
		{
			sort_element_swap(++p, i);
		}
	}
	sort_element_swap(p, beg);
	return p;
}

void quick_sort_recursive(sort_element_t * beg, sort_element_t * end)
{
	if (end - beg > 1)
	{
		sort_element_t * p = partition(beg, end);
		quick_sort_recursive(beg, p);
		quick_sort_recursive(p + 1, end);
	}
}

void quick_sort(sort_element_t * arr, size_t len)
{
	quick_sort_recursive(arr, arr + len);
}
```
</details>

以上为三种快速排序的基本实现，也是网上常见的版本

而其中第二种有非常多的变种，陷阱也是最多的

这三种都有相同的缺陷，对完全相同的序列特别慢，甚至会发生错误，不应在应用场景中使用

在应用场景下使用快排，必须使用其优化版本

</details>

<details>
<summary>写法8 快速排序优化</summary>

所谓优化，主要是为了让分割更分均匀，减少出现最坏情况的可能

<details>
<summary>优化 1</summary>

以下优化代码基于前面的实现2修改而来，分割数使用随机选择

分割数组时严格分割为小于pivot和大于等于pivot的两段的写法，并过滤相等的数据

在大量相同数据下也几乎不会陷入最坏情况

```c
void sort_element_swap(sort_element_t *x, sort_element_t *y)
{
	sort_element_t t = *x;
	*x = *y;
	*y = t;
}

sort_element_t* quick_sort_partition_rand3(sort_element_t * begin, sort_element_t * end, sort_element_t * p_pivot)
{
	static uint32_t s_rnd = 0xffffff;
	sort_element_swap(begin + (++s_rnd) % (end - begin), begin);
	sort_element_t pivot = *p_pivot = *begin;
	sort_element_t *l = begin, *r = end - 1;

	while (l < r)
	{
		while (l < r && !(*r < pivot))
			r--;
		while (*l < pivot)
			l++;
		if (l < r)
			sort_element_swap(l, r);
	}
	return l;
}

void quick_sort_recursive_rand3(sort_element_t * begin, sort_element_t * end)
{
	if (begin + 1 < end)
	{
		sort_element_t pivot, *p = quick_sort_partition_rand3(begin, end, &pivot);
		quick_sort_recursive_rand3(begin, p);
		while (p < end && !(*p < pivot) && !(pivot < *p)) ++p;
		quick_sort_recursive_rand3(p, end);
	}
}

void quick_sort_rand3(sort_element_t arr[], size_t n)
{
	quick_sort_recursive_rand3(arr, arr + n);
}
```

</details>

<details>
<summary>优化 2</summary>

基于前一个代码，记录递归深度，在深度过大的时候转而使用堆排序

这样完全避免在非常特殊的数据下运行时间过长或堆栈溢出

从而让这个实现成为实用级别，保证最坏情况下的算法复杂度是O(nlogn)

这个算法叫做内省排序（Introsort）

代码需要引用堆排序，请自行添加

```c
sort_element_t* intro_sort_partition_rand(sort_element_t * begin, sort_element_t * end, sort_element_t * p_pivot)
{
	static uint32_t s_rnd = 0xffffff;
	sort_element_swap(begin + (++s_rnd) % (end - begin), begin);
	sort_element_t pivot = *p_pivot = *begin;
	sort_element_t *l = begin, *r = end - 1;

	while (l < r)
	{
		while (l < r && !(*r < pivot))
			r--;
		while (*l < pivot)
			l++;
		if (l < r)
			sort_element_swap(l, r);
	}
	return l;
}

void intro_sort_recursive_1(sort_element_t * begin, sort_element_t * end, int depth)
{
	if (begin + 1 >= end)
		return;

	if (depth <= 0)
	{
		heap_sort(begin, end);
		return;
	}

	sort_element_t pivot, *p = intro_sort_partition_rand(begin, end, &pivot);

	intro_sort_recursive_1(begin, p, depth - 1);
	while (p < end && !(*p < pivot) && !(pivot < *p)) ++p;
	intro_sort_recursive_1(p, end, depth - 1);
}

void intro_sort_1(sort_element_t arr[], size_t n)
{
	int depth = (int)(log(n) * 2.5);
	intro_sort_recursive_1(arr, arr + n, depth);
}
```

</details>

快排的基本优化介绍到这里，关于快排仍有很多细节可优化，但这里不展开讨论了

</details>

<details>
<summary>写法9 堆排序</summary>

```c
void max_heapify(sort_element_t arr[], size_t index, size_t length)
{
	size_t child;
	sort_element_t temp = arr[index];
	for (; (child = 2 * index + 1) < length; index = child)
	{
		if (child + 1 < length && !(arr[child + 1] < arr[child]))
			++child;
		if (temp < arr[child])
			arr[index] = arr[child];
		else
			break;
	}
	arr[index] = temp;
}

void heap_sort(sort_element_t arr[], size_t length)
{
	if (length > 1)
	{
		for (size_t i = length / 2 - 1; i > 0; --i)
			max_heapify(arr, i, length);
		max_heapify(arr, 0, length);
		for (size_t i = length - 1; i > 0; --i)
		{
			sort_element_swap(&arr[0], &arr[i]);
			max_heapify(arr, 0, i);
		}
	}
}
```
</details>

<details>
<summary>写法10 归并排序</summary>

```c
void merge_sort_recursive(sort_element_t buf[], sort_element_t * beg, sort_element_t * end)
{
	size_t len = end - beg;
	if (len < 2)
	{
		return;
	}
	sort_element_t * mid = beg + len / 2;
	merge_sort_recursive(buf, beg, mid);
	merge_sort_recursive(buf, mid, end);

	sort_element_t * start1 = beg, *start2 = mid, *k = buf;
	while (start1 < mid && start2 < end)
		*k++ = !(*start2 < *start1) ? *start1++ : *start2++;
	while (start1 < mid)
		*k++ = *start1++;
	while (start2 < end)
		*k++ = *start2++;

	for (k = buf; beg < end; k++, beg++)
		*beg = *k;
}

void merge_sort(sort_element_t arr[], size_t len)
{
	sort_element_t* buf = (sort_element_t*)malloc(len * sizeof(*arr));
	merge_sort_recursive(buf, arr, arr + len);
	free(buf);
}
```
</details>

<details>
<summary>写法11 基数排序</summary>

要注意的是此算法不是基于比较的排序

而且此实现只能对非负整数排序
```c
typedef int radix_index_t;
const radix_index_t radix_sort_base = 16;

radix_index_t _radix_get_index(sort_element_t* p_data)
{
	return (radix_index_t)*p_data;
}

size_t maxbit(sort_element_t data[], size_t n)
{
	radix_index_t maxData = _radix_get_index(data);
	for (size_t i = 1; i < n; ++i)
	{
		if (maxData < _radix_get_index(data + i))
			maxData = _radix_get_index(data + i);
	}
	size_t d = 1;
	radix_index_t p = radix_sort_base;
	while (maxData >= p)
	{
		maxData /= radix_sort_base;
		++d;
	}
	return d;
}

void radix_sort(sort_element_t data[], size_t n)
{
	if (n < 2)
	{
		return;
	}
	size_t d = maxbit(data, n);
	sort_element_t *tmp = (sort_element_t *)malloc(n * sizeof(sort_element_t));
	size_t *count = (size_t *)malloc(radix_sort_base * sizeof(size_t));
	size_t j;
	radix_index_t radix = 1;
	for (size_t sort_cnt = 0; sort_cnt < d; ++sort_cnt) //进行d次排序
	{
		memset(count, 0, radix_sort_base * sizeof(size_t));

		for (j = 0; j < n; ++j)
		{
			radix_index_t bucket = (_radix_get_index(data + j) / radix) % radix_sort_base; //统计每个桶中的记录数
			++count[bucket];
		}
		for (radix_index_t bucket_i = 1; bucket_i < radix_sort_base; ++bucket_i)
			count[bucket_i] = count[bucket_i - 1] + count[bucket_i];

		for (j = n - 1; ; --j) //将所有桶中记录依次收集到tmp中
		{
			radix_index_t bucket = (_radix_get_index(data + j) / radix) % radix_sort_base;
			tmp[--count[bucket]] = data[j];
			if (j == 0) break;
		}
		for (j = 0; j < n; ++j) //将临时数组的内容复制到data中
			data[j] = tmp[j];
		radix *= radix_sort_base;
	}
	free(tmp);
	free(count);
}
```
</details>

还有更多的排序方法这里不一一介绍了，详见维基百科[排序算法](https://en.wikipedia.org/wiki/Sorting_algorithm)

另外，排序算法的运行时间比较可以参阅 [这里](sort_compare.md)