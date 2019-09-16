# 第三章——条件分支与逻辑运算

## if 语句

这个语句非常重要而且非常有重，程序里到处都能看到它的影子，那这个有什么用处呢，我们来做一个小函数作为示例，这里我们写一个求绝对值的函数。

```c
#include<stdio.h>

int abs(int n)
{
    if (n < 0)
    {
        return -n;
    }
    return n;
}

int main(void)
{
    int a = 2, b = -3;
    printf("%d %d\n", abs(a), abs(b));
    return 0;
}
```

这里我们可以看到，if语句的格式为（格式1）

```
if (条件)
{
    如果条件成立就执行这里的代码
}
如果条件不成立就跳到这里
```

那什么叫做条件成立呢，例如我们刚才写的，`n < 0` 就是一个比较大小的条件，如果n是-1或-2，那就条件成立了，会执行if后面由一对大括号括起来的代码。但是如果那个n是0或1，那条件就不成立了，结果是后面大括号括起来的代码全部跳过不理。以上是if语句的第一种格式，第二种格式看以下例子

```c
int abs(int n)
{
    if (n < 0)
    {
        return -n;
    }
    else
    {
        return n;
    }
}
```

比起前一个就是多了个else，相当于通常所理解的“否则”。那个条件成立，就执行前一块，不成立就执行下面一块。if语句的格式2

```
if (条件)
{
    如果条件成立就执行这里的代码
}
else
{
    如果条件不成立就跳到这里
}
其它语句
```

不过，如果条件更为复杂怎么办呢，来看这个情况，编写一个函数，输入一个分数，输出这个分数所属等级，定义85-100为1级，70-84为2级，60-69为3级，0-59为4级，其它分数为0级。面对再复杂的条件，我们也要一步一步来，先写函数定义

```c
int score_rank(int score)
{

}
```

然后，我们从大到小考虑，先是如果大于100的情况，写出分支

```c
int score_rank(int score)
{
    if (score > 100)
    {
        return 0;
    }
    else
    {

    }
}
```

再接着，是85到100这个分数段，我们知道，在上面代码里如果分数大于100是走了return 0，否则就走else，那我们在else的里面再做判断

```c
int score_rank(int score)
{
    if (score > 100)
    {
        return 0;
    }
    else
    {
        if (score >= 85)
        {
            return 1;
        }
        else
        {

        }
    }
}
```

以上在if语句里面再写if语句的方式，叫做嵌套。这样，里面虽然只判断了`if (score >= 85)`，但由于在外面一层必须满足`score <= 100`才会进入else里面，所以相当于限制了score的范围是85到100才返回1。接下来后面的做法相似，代码如下

```c
int score_rank(int score)
{
    if (score > 100)
    {
        return 0;
    }
    else
    {
        if (score >= 85)
        {
            return 1;
        }
        else
        {
            if (score >= 70)
            {
                return 2;
            }
            else
            {
                if (score >= 60)
                {
                    return 3;
                }
                else
                {
                    if (score >= 0)
                    {
                        return 4;
                    }
                    else
                    {
                        return 0;
                    }
                }
            }
        }
    }
}
```

到这里如果还是不太清楚if语句的执行次序，请看下一章的代码执行次序与调试篇。看到这里，有没有觉得这个代码很繁琐？确实，有点太长，不过没有关系，我们来缩短它，让它又短又好懂。首先，else语句里面如果只有一个语句，是可以不需要写大括号的，例如里面只有一个`return 0;`，这时候可以省略大括号。与此同时，整个if-else，外面也算作一个语句，所以可以缩略成这样

```c
int score_rank(int score)
{
    if (score > 100)
    {
        return 0;
    }
    else
    {
        if (score >= 85)
        {
            return 1;
        }
        else
        {
            if (score >= 70)
            {
                return 2;
            }
            else
            {
                if (score >= 60)
                {
                    return 3;
                }
                else //注意这里1
                    if (score >= 0)
                    {
                        return 4;
                    }
                    else  //注意这里2
                        return 0;
            }
        }
    }
}
```

要注意的点已经在上面代码用注释标记上。那我们再多省略一点吧

```c
int score_rank(int score)
{
    if (score > 100)
    {
        return 0;
    }
    else
        if (score >= 85)
        {
            return 1;
        }
        else
            if (score >= 70)
            {
                return 2;
            }
            else
                if (score >= 60)
                {
                    return 3;
                }
                else
                    if (score >= 0)
                    {
                        return 4;
                    }
                    else
                        return 0;
}
```

代码确实短了一些，不用滚动那么多行，但看起来还是有点不舒服，我们还可以把else与下一行的if连着写在一行上

```c
int score_rank(int score)
{
    if (score > 100)
    {
        return 0;
    }
    else if (score >= 85)
        {
            return 1;
        }
        else if (score >= 70)
            {
                return 2;
            }
            else if (score >= 60)
                {
                    return 3;
                }
                else if (score >= 0)
                    {
                        return 4;
                    }
                    else
                        return 0;
}
```

还是有点不舒服，我们再调整一下缩进

```c
int score_rank(int score)
{
    if (score > 100)
    {
        return 0;
    }
    else if (score >= 85)
    {
        return 1;
    }
    else if (score >= 70)
    {
        return 2;
    }
    else if (score >= 60)
    {
        return 3;
    }
    else if (score >= 0)
    {
        return 4;
    }
    else
        return 0;
}
```

现在看起来就好多了，代码变得简洁易懂多了，不过要注意这里实质上是在语法允许的情况下，通过省略大括号并调整书写代码方式得到的，有的书单独把else if写成一种if语句格式，虽然这么理解是等价的，但写出这个是希望你能理解else if是怎么来的。通过if-else语句和它的嵌套，就能写出任意的逻辑组合，而且不需要下文提及的逻辑运算符，当然有逻辑运算符作辅助能让逻辑更清晰。

## switch 语句

这个语句这里只提及一下，switch的功能完全能被if语句替代，只有少数场合有使用switch的价值，所以这里只提及不介绍，后面也不会使用上这个函数，有兴趣可以自行查阅资料。

## 逻辑运算符

我们来实现这样一个函数

```c
int in_range(int n, int low, int high)
{

}
```

这函数要干的事是判断n是不是在low和high之间，如果是就返回1，否则返回0。我们如果只用if-else可以写出如下代码

```c
int in_range(int n, int low, int high)
{
    if (n > high)
    {
        return 0;
    }
    else if (n >= low)
    {
        return 1;
    }
    else
    {
        return 0;
    }
}
```

确实能实现没错，但我们能不能写得更为直观点，怎么表达n在两个数之间呢？注意，不能写成数学的方式例如 `low <= n <= high` ，已经多次强调不要套用数学上的式子。注意的是，这里实质上是要求`low <= n`和`n <= high`同时成立，这里c语言有 `&&` 运算符，要求两边表达式同时成立时，整个式子才成立，可以把代码改成如下

```c
int in_range(int n, int low, int high)
{
    if (low <= n && n <= high)
    {
        return 1;
    }
    else
    {
        return 0;
    }
}
```

又或者，c语言有 `||` 运算符，要求两边表达式只要有一个成立，整个式子就成立，可以把代码改成如下

```c
int in_range(int n, int low, int high)
{
    if (n < low || high < n)
    {
        return 0;
    }
    else
    {
        return 1;
    }
}
```

c语言里还有一个 `!` 运算符，表示它右边的表达式取相反的结果，示例如下

```c
int in_range(int n, int low, int high)
{
    if (!(low <= n) || !(n <= high))
    {
        return 1;
    }
    else
    {
        return 0;
    }
}
```

也就是说，`!(low <= n)` 相当于 `low > n`

以上三个基本逻辑运算符就介绍到这里
