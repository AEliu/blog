# TCPL

标签（空格分隔）： C语言

---
[TOC]


----------


## 导言


----------


### 入门
作者在入门这一节里引入了打印`hello, world`的一段程序，这变成了程序设计教材里的榜样。
```bash
# 编译
cc hello.c 
```
字符串中的转义字符序列：

| 名称 | 字符 |
| :--: | :--: |
| 换行 | \n |
| 制表符 | \t |
| 回退符 | \b |
| 双引号 | \" |
| 反斜杠 | \\\ |

### 变量与算术表达式

 1. 变量必须先声明后使用
 2. 不同变量类型的取值范围与机器有关
 3. 语句写成`celsius = 5 * (fahr - 32) / 9;` 而不是`celsius = 5 / 9 * (fahr - 32);` 是因为整数除法执行舍位，小数部分被舍弃。
 4. `%6d` 数字至少占6字符宽且右对齐，`%6.1f` 浮点数两位小数至少占6字符宽且右对齐
 5. 格式化输出：
 
| 格式 | 输出形式 |
| :---: | :---: |
| %o | 八进制 |
| %x | 十六进制 |
| %c | 字符 |
| %s | 字符串 |
| %% | %本身 |

### for语句
比`while`语句紧凑

### 符号常量
**不要使用幻数**
```C
#define 名字 替换文本
/*注意没有分号*/
```

### 字符输入/输出

 1. 字符输入/输出函数是`getchar`/`putchar`
 2. `c = getchar()`读入下一个输入字符，并返回
 3. `putchar(c)`
 
#### 文件复制
 1. macos的EOF是`control + d`
 2. 记住`while ((c = gentchar()) != EOF`
 
#### 字符计数
`++nc;`

#### 行计数
```C
if (c == '\n')
    ++nl;
```

#### 单词计数
```C
#include <stdio.h>

#define IN 1
#define OUT 0 
// 这俩比较好，避免了幻数

int main()
{
    int c, nl, nw, nc, state;

    state = OUT; // 刚开始在单词外面
    nl = nw = nc = 0;

    while((c = getchar()) != EOF) {
        ++nc;
        if (c == '\n')
            ++nl;
        if (c == ' ' ||  c == '\n' || c == '\t') {
            state = OUT; //空白字符就在单词外了
        } else if (state == OUT) { 
        //如果不是在单词外，又不是空白字符，那只能是刚进入单词了
            ++nw;
            state = IN; // 于是状态改变
        }
    }
    printf("%d %d %d\n", nl, nw, nc);
    
    return 0;
}
```

### 数组
for循环与数组，数组序号从0开始

### 函数

 1. 函数的定义可以以任意次序出现在一个或多个源文件
 2. 定义函数时的参数是形式参数
 
### 参数-传值调用
 1. 函数参数都是通过值传递，被调用的函数不能直接修改主调函数中变量的值
 2. 除非参数是地址（数组名、指针等）
 
### 字符数组
字符串常量以数组形式存储时以`\0`标记结束

一个字符数组复制程序：
```C
void copy(char to[], char from[]) // 字符数组做参数，还要加上[]
{
    int i;
    i = 0;
    while ((to[i] = from[i]) != '\0')
        ++i;
        // 没有return， 返回void
}
```

### 外部变量与作用域
 1. 局部变量就是自动变量，函数执行完毕退出时消失
 2. 外部变量必须定义在所有函数之外，且只能定义一次，一般反正源文件的开始之处
 3. 函数需要访问外部变量时，需要进行声明，可以用`extern`显式声明，如果外部变量的定义在使用它的函数的源文件之前，可以省略
 4. 如果是在其他的文件定义外部变量，那么使用之前需要声明，一般把变量与函数的声明放在头文件（`*.h`）里，然后`#include`进来
 
----------

## 类型、运算符与表达式


----------
### 变量名

 1. 字母、数字、下划线，一般以字母开头，大小写区分
 2. 关键字不能作为变量名
 
### 数据类型及长度

| 数据类型 | 长度 |
| :------: | :--: |
| char | 1字节 |
| int | 机器中最自然长度 |
| long int | 至少32位 |
| short int | 至少16位 |
| float |  |
| double |  |
| long double |  |

### 常量
1. 前缀、后缀或表示形式
| 常量类型 | 后缀(前缀)、形式 |
| :------: | :--: |
| long int | l or L |
| unsigned long | ul or UL |
| float | f or F |
| long double | l or L |
| 八进制 | 0 |
| 十六进制 | 0x |
| 字符常量 | 'a' |
| 转义字符 | eg. '\n' |
| null空字符 | '\0' |
| 字符串常量 | "abcde" |
| 枚举常量 | `enum boolean { NO, YES }` |
2. 利用类似`#define BELL '\007'`
3. 第2条的常量在编译时求值
4. `strlen(s)`返回s的长度，但没有最后的`'\0'`
5. 枚举的第一个枚举名的值是0，依次递增，也可以自行指定值
6. 枚举的第一个枚举名的值指定为1，后面未指定，则第二个为2，依次递增

### 声明
 1. 所有变量必须先声明后使用
 2. 声明的同时可以进行初始化
 3. 默认情况下，外部变量与静态变量被初始化为0
 4. 声明可以用const限定符限定，其之后不能修改
 
### 算术运算符
见 [运算符优先级与求值次序][1]

### 关系运算符与逻辑运算符
1. &&、||从左到右进行求值，得到真或假后立即停止
2. 逻辑非一般采用`if (!valid)` 不采用 `if (valid == 0)`

### 类型转换
1. 自动转换，窄的转换成宽的，比如int自动转换成float
2. char类型变量没有指定是signed还是unsigned，转换为int时在某些机器可能为负整数
3. “真”意味着“非0”
4. float类型不会自动转换为double类型
5. unsigned类型转换规则没有看懂
6. 较长的整数转换为较短的整数或char类型时，超出的高位部分将被丢弃
7. float转换为int类型，小数部分被截取掉
8. 没有函数原型的情况下，char与short类型都将被转换成int类型，float被转换成double类型，因此即使调用的函数的参数为char或float, 我们也把函数参数声明为int或double类型
9. 强制转换`(double) i;`
10. 参数是通过函数原型声明的，函数被调用时声明对参数进行自动转换，如函数原型为`double sqrt(double);`，调用时`root2 = sqrt(2);`，其中`2`强制为double类型`2.0`

### 自增运算符与自减运算符
```
n = 5;
x = n++; // x == 5
n = 5;
y = ++n; // y == 6
```
删除字符串s中所有字符c的程序squeeze（记住）:
```C
void suqeeze (char s[], int c)
{
    int i, j;
    for (i = j =0; s[i] != '\0'; i++)
        if (s[i] != c)
            s[j++] = s[i]; 
    s[j] = '\0'
}
```

### 按位运算符[^footnote]
1. C语言提供6个位运算符
|运算符 |名称   |
|:-----:|:-----:|
| &  |按位与（AND）|
| \| |按位或（OR）|
| ^  |按位异或（XOR）|
| << |左移|
|\>> |右移|
| ~  |按位求反（一元运算符）|
2. 位运算符只能作用于`char`、`short`、`int`、`long`
3. 将n中除7个低二进制位外其他各位设置为0: `n = n & 0177 // 8进制0177` 
4. 左移空出来的位用0填充
5. 右移unsigned类型无符号值，左边空出来的位用0填充，右移signed类型无符号值，左边空出来的算术移位或逻辑移位。
6. 右移后，左边空位用符号位填补为算术移位，用0填补为逻辑移位
7. 将n中最后6个二进制位设置为0: `n = n & ~077 // 8进制077`
8. 返回x从右边数第p位开始向右n位的字段：
```c
unsigned getbits (unsigned x, int p, int n)
{
    return (x >> (p+1-n)) & ~(~0 << n);
    // x >> (p+1-n) 将期望得到的字段移到最右端
    // ~0所有位都是1，~0 << n最右边的n位用0填充，~后0 —› 1
    // 最后保留最右端的n位
}
```
9. 异或当对应位不同时该位为1，相同时该位为0，因此^0仍为本身，^1为取反
10. 练习2-8未做



### 赋值运算符与表达式
统计整型参数的值为1的二进制位个数
```c
int bitcount(unsigned x)
{
    int b;
    for (b = 0; x != 0; x >>= 1)
        if (x & 01)
            b++;
    return b;
}
```

### 条件表达式
1. 打印数组，每列用空格隔开，每行10个元素：
```c
for (i = 0; i < n; i++)
    printf("%6d%c", a[i], (i%10==9 || i == n-1) ? '\n' : ' ');
```
2. 多于1，名词复数形式：
```c
printf("You have %d item%c.\n", n == 1 ? '' : 's');
```

### 运算符优先级与求值次序

---
## 控制流
---
### 语句和程序块

### if-else语句
每个else与最近的前一个没有else配对的if进行匹配

### else-if语句
折半查找：
```c
int binsearch(int x, int v[], int n)
{
    int low, high, mid;

    low = 0;
    high = n - 1;

    while(low <= high) {
        mid = (high - low) / 2;
        if (x < v[mid])
            high = mid - 1;
        else if (x > v[mid])
            low = mid + 1;
        else   
            return mid;
    }

    return -1;
}
```

### switch语句
```c
switch (表达式) {
    case 常量表达式: 语句序列
    case 常量表达式: 语句序列
    default : 语句序列 // 可选分支，后面加上break语句
}
```
依次执行各分支，因此分支后必须加上break语句结束

### while循环与for循环
* 语句中有简单的初始化和变量递增，使用for语句更合适
* 带符号的字符串转换为数值：
```c
#include <ctype.h>

int atoi(char s[])
{
    int i, n, sign;

    for (i = 0; isspace(s[i]); i++)
        ;
    sign = (s[i] == '-') ? -1 : 1;

    if (s[i] == '-' || s[i] == '+')
        i++;

    for (n = 0; isdigit(s[i]); i++)
        n = 10 * n + (s[i] - '0');

    return sign * n;
}
```
* shell排序 - 没懂，继续
```c
void shellsort(int v[], int n)
{
    int gap, i, j, temp;

    for (gap = n/2; gap > 0; gap /=2)
        for (i = gap; i < n; i++)
            for(j = i - gap; j >= 0 && v[j] > v[j+gap]; j -= gap) {
                temp = v[j];
                v[j] = v[j + gap];
                v[j+gap] = temp;
            }
}
```
* 逗号运算符“,”是c语言中优先级最低的运算符，逗号运算符分割的一堆表达式从左到右求值，
* 倒转字符串s内各字符的位置
```c
#include <string.h>

void reverse (char s[])
{
    int c, i, j;

    for (i = 0, j = strlen(s) - 1; i < j; i++, j--)
        c = s[i], s[i] = s[j], s[j] = c;
}
```
### do-while循环
* 数字n转换为字符串并保存到2中
```c
void itoa (int n, char s[])
{
    int sign, i;

    if ((sign = n) < 0)
        n = -n;
    i = 0
    do {
        s[i++] = n % 10 + '0';
    } while ((n / 10) > 0);
    if (sign < 0)
        s[i++] = '-';
    s[i] = '\0';
    reverse(s);
}
```
### break语句与continue语句
* trim函数：删除字符串尾部的空格符、制表符与换行符
```c
int trim(char s[])
{
    int n;

    for (n = strlen(s) - 1; n >= 0; n--)
        if (s[n] != ' ' && s[n] != '\n' && s[n] != '\t')
            break;
    s[n+1] = '\0';
    return n;
}
```
### goto语句与标号

---
## 函数与程序结构
---
### 函数的基本知识
* 如果函数定义时省略了返回值，默认为int类型
* getline函数`int getline(char line[], int max);`，将行保存在s中，并返回该行长度
```c
int getline(char line[], int max);

int getline(char s[], int lim)
{
    int c, i;

    i = 0;
    while (--lim > 0 && (c = getchar()) != EOF && c != '\n')
        s[i++] = c;
    if (c == '\n')
        s[i++] = c;
    
    s[i] = '\0';

    return i;
}
```
* strindex函数`int strindex(char source[], char searchfor[]);`，返回t在s中的位置，未找到返回-1
```c
int strindex(char source[], char searchfor[]);

int strindex(char s[], char t[])
{
    int i, j, k;

    for (i = 0; s[i] != '\0'; i++) {
        for (j = i, k = 0; t[k] == s[j] && t[k] != '\0'; j++, k++)
            ;
        if (k > 0 && t[k] == '\0')
            return i;
    }

    return -1;
}
```
### 返回非整型的函数
* 把字符串s转换成相应的double
```c
#include <ctype.h>

double atof (char s[])
{
    int i, sign;
    double val, power;

    for (i = 0; isspace(s[i]); i++)
        ;
    sign = (s[i] == '-') ? -1 : 1;
    if (s[i] == '+' || s[i] == '-')
        i++;
    for (val = 0.0; isdigit(s[i]); i++)
        val = val * 10.0 + (s[i] - '0');
    if (s[i] == '.')
        i++;
    for (power = 1.0; isdigit(s[i]); i++) {
        val = val * 10.0 + (s[i] - '0'); // power的引入使得这个继续
        power *= 10.0;
    }

    return sign * val / power;
}
```
* 把字符串s转换成相应的int（利用atof）
```c
int atoi(char s[])
{
    double atof(chars[]);
    return (int) atof(s);
}
```




  [1]: ###%20%E8%BF%90%E7%AE%97%E7%AC%A6%E4%BC%98%E5%85%88%E7%BA%A7%E4%B8%8E%E6%B1%82%E5%80%BC%E6%AC%A1%E5%BA%8F
  [^footnote]: C语言的数据如何表示？ 需要另行查找资料。
