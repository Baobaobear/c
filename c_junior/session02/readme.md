# 第二章——表达式，变量，函数

## 表达式

作为计算机，当然不可能要自己去计算，看看这个例子

```c
#include<stdio.h>
int main(void)
{
    printf("%d", 1*2*3*4*5*6*7*8*9);
    return 0;
}
```

运行输出 362880 ，我们没有必要自己算出结果填上去，而 `1*2*3*4*5*6*7*8*9` 就是表达式，这个概念就这么简单。

## 变量

```c
#include<stdio.h>
int main(void)
{
    int val;
    val = 1*2*3*4*5*6*7*8*9;
    printf("%d", val);
    return 0;
}
```

看这个例子，其中 `int val;` 这一行，就是定义一个整数变量，格式是

```
类型名 变量名;
类型名 变量名1, 变量名2, ...;
```

可以同时定义多个变量名字。 `val = 1*2*3*4*5*6*7*8*9;` 这步，是给变量val赋值，等于号是赋值运算符，左边是要改变的变量，右边是表达式，这个命令执行完后，左边变量的值就会变成右边表达式的值了，最后再输出变量val的值。再来看一个复杂点的例子

```c
#include<stdio.h>
int main(void)
{
    double val;
    val = 1.0;
    val = val / 7.0;
    printf("%g\n", val);
    val = val * 14;
    printf("%g\n", val);
    return 0;
}
```

注意的是，看代码要从前面开始，你要忘掉你的数学的经验，**计算机只会按你写的代码一步一步操作**。

首先，定义了变量val，类型是double，double表示的是双精度浮点类型，反正你记得是浮点数就行了。

然后，让val的值是1，这里有一点，1.0和1在数值上是相同的，但类型上不同，1是整数，1.0是浮点数(double)，于是这个时候，val就是1

接着，`val = val / 7.0;` ，右边表达式相当于是计算1除以7，计算结果应该是约等于0.142857，后面就立即输出了这个数，确实如此。

最后，`val = val * 14;` ，val再乘以14，那么应该就是2了，就看第二个输出确认一下。这里，val是个double，但14是的int类型的整数，这样会发生什么呢，这涉及到类型表示范围的问题。int在Visual Studio上，表示范围是 -2147483648 ~ 2147483647 ，而 2147483648等于2的31次方。而double表示的范围大得多，可以表示10的308次方，但精度约只有15位小数，不过也足够表示int的范围，所以int与double混合运算的时候，规定把int转换为double再运算，所以结果的类型一定是double。

## 函数

函数是什么呢？其实函数是个无处不在的东西，前面一直没有提到但很重要，我们还是拿 hello world 做例子

```c
#include<stdio.h>

int main(void)
{
    printf("Hello world");
    return 0;
}
```

其中，`int main(void)`这叫做函数头，后面由一对 {} 括起来的叫函数体，特别的，main这个函数叫做**主函数**，也是整个程序最先执行的地方。为了让大家更好的理解，我们来编写一个求一个数的平方的函数


```c
#include<stdio.h>

int sqr(int n)
{
    return n * n;
}

int main(void)
{
    int val = 12, s;
    s = sqr(val);
    printf("sqr %d = %d\n", val, s);
    return 0;
}
```

以上，`int sqr(int n)`就是函数头，最前面的int表示函数返回值，或者理解为函数值。sqr是你定义的函数名，后面括号里面是参数列表，如果有多个参数，就用逗号分隔。如果不需要参数，可以不写，或者写void强调没有参数。

而函数体就是你想要执行的一系列操作，会从函数体第一行一直执行到最后一行，或者遇到return语句中止，return语句后面可以跟一个表达式，表示要返回的函数值，然后执行的位置回到调用函数的地方，由于这里包含了执行路线的跳转，所以 return返回 应该是挺形象的。

所以在执行 `s = sqr(val);` 的时候，先要计算 `sqr(val)` 是多少，那么参数val的值传入，跳到函数sqr里面，这个时候在sqr函数里的n的值，就等于外面传入的值12，然后执行 `return n * n;` 计算出n*n等于144，于是返回值是144。然后回到主函数main里面，这时候相当于是 `s = 144;` ，想象出把函数计算后的值替换掉原来调用的位置。于是s的值就是144。

更深入的内容会在后面章节一点点介绍