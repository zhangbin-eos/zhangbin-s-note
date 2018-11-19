
# GUN C 规范和特性


# 关键字

#### C89
```
auto break case char const continue default do double else enum extern
float for goto if int long register return short signed sizeof static
struct switch typedef union unsigned void volatile while
```
- register 关键字可以在全局中定义变量,当对其变量使用 & 操作符时,只是警告“有坏的存储类”;可以在局部作用域中声明,但这样就无法对其使用 & 操作符.否则编译不通过

#### ISO C99
```
inline _Bool _Complex _Imaginary
```
-  inline 内联
-  _Bool 布尔
-  _Complex 复数
-  _Imaginary 虚数

#### GNU
```
__FUNCTION__ __PRETTY_FUNCTION__ __alignof __alignof__ __asm
__asm__ __attribute __attribute__ __builtin_offsetof __builtin_va_arg
__complex __complex__ __const __extension__ __func__ __imag __imag__
__inline __inline__ __label__ __null __real __real__
__restrict __restrict__ __signed __signed__ __thread __typeof
__volatile __volatile__
```
- `__asm` 内嵌汇编
- `__thread`线程全局变量,每个线程具有单独拷贝,只能修饰POD类型;`__thread` 修饰的变量,在线程中地址都不一样
- `__attribute__` GNU C 语言属性,`__attribute__` 可以设置函数属性(Function Attribute)、变量属性(Variable Attribute)和类型属性(Type Attribute);参考[__attribute__详解](gnu-c/attribute.md)

#### both
```
restrict
```
- restrict 优化指针,标记这个指针指向的对象只能通过这个指针进行修改
# 变量

## int

### 初始化

### 位域
1. 一个位域必须存储在同一个单元中，不能跨两个单元。如一个单元所剩空间不够存放另一位域时，应从下一单元起存放该位域。也可以有意使某位域从下一单元开始。位域可以无位域名，这时它只用来作填充或调整位置。无名的位域是不能使用的 例如：
```
struct bs
{
       unsigned a:4
       unsigned :0
       unsigned b:4
       unsigned c:4
       int :2
}
```
这个位域定义中，a占第一字节的4位，后4位填0表示不使用，b从第二字节开始，占用4位，c占用4位。

# 函数

## 变长参数列表函数(std)
```
NAME
       stdarg, va_start, va_arg, va_end, va_copy

SYNOPSIS
       #include <stdarg.h>

       void va_start(va_list ap, last);
       type va_arg(va_list ap, type);
       void va_end(va_list ap);
       void va_copy(va_list dest, va_list src);
```
1. 函数必须有至少1个固定类型的变量
2. 必须指定或者标记函数的变长参数列表的长度或者结束位置,比如printf的参数个数通过fmt字符串来进行判断
3. va_start va_end成对出现,否则会出现段错误



<meta http-equiv="refresh" content="1">