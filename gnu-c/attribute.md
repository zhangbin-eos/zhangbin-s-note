# 概述

`__attribute__` 可以设置函数属性（Function Attribute）、变量属性（Variable Attribute）和类型属性（Type Attribute）。

其位置约束为：  放于声明的尾部“;” 之前

`__attribute__` 书写特征为:  `__attribute__`前后都有两个下划线，并切后面会紧跟一对原括弧，括弧里面是相应的`__attribute__`参数。

`__attribute__`语法格式为:  `__attribute__((attribute-list))`


# 函数属性（Function Attribute)
函数属性可以帮助开发者把一些特性添加到函数声明中，从而可以使编译器在错误检查方面的功能更强大。__attribute__机制也很容易同非GNU应用程序做到兼容之功效。GNU CC需要使用 –Wall编译器来击活该功能，这是控制警告信息的一个很好的方式。

下面介绍几个常见的属性参数:

##`__attribute__ format`
  该属性可以给被声明的函数加上类似printf或者scanf的特征，它可以使编译器检查函数声明和函数实际调用参数之间的格式化字符串是否匹配。该功能十分有用，尤其是处理一些很难发现的bug。

format的语法格式为：format (archetype, string-index, first-to-check)

 

format属性告诉编译器，按照 printf, scanf, strftime或strfmon的参数表格式规则对该函数的参数进行检查。“archetype”指定是哪种风格；“string-index”指定传 入函数的第几个参数是格式化字符串；“first-to-check”指定从函数的第几个参数开始按上述规则进行检查。

 

具体使用格式如下：
```
__attribute__((format(printf,m,n)))
__attribute__((format(scanf,m,n)))
```
 
其中参数m与n的含义为：
m：第几个参数为格式化字符串（format string）；
n：参数集合中的第一个，即参数“…”里的第一个参数在函数参数总数排在第几.这里需要注意，有时函数参数里还有“隐身”的，如C++的类成员函数的第一个参数实际上是"隐身"的"this"指针；

在使用上，`__attribute__((format(printf,m,n)))`是常用的，而另一种却很少见到。

下面举例说明，其中myprint为自己定义的一个带有可变参数的函数，其功能类似于printf：
```
// m = 1, n = 2...如果在这里myprint()为类成员函数,则gcc编译后会提示`format argument is not a pointer`的警告
extern void myprint(const char *format,...) __attribute__((format(printf,1,2)));

// m = 2, n = 3
extern void myprint(short num，const char *format,...) __attribute__((format(printf,2,3)));
```
 
最经典的应用可以去看linux源码里的函数`device_create(...)`和`class_device_create(...)`, 做linux驱动开发的小伙伴应该对这两个函数不陌生.

##`__attribute__ noreturn`
该属性通知编译器函数从不返回值。当遇到函数需要返回值却还没运行到返回值处就已退出来的情况，该属性可以避免出现错误信息。

C库函数中的abort()和exit()的声明格式就采用了这种格式：
```
extern void exit(int)  __attribute__((noreturn));
extern void abort(void)  __attribute__((noreturn));
```
例如下面这段代码:
```
extern void myexit(int);

int testFunc(void)
{
    printf("-- Enter %s --", __func__);

    myexit(0);

    // 其实函数运行不到这里
    printf("-- Exit %s --", __func__);
}

void myexit(int i)
{
    exit(i);
}

```
编译时会报"control reaches end of non-void function"的警告, 但若将"extern void myexit(int);"改为"extern void myexit(int) __attribute__((noreturn));" 就不会再报警告了.
## `__attribute__ constructor/destructor`
若函数被设定为constructor属性，则该函数会在 main（）函数执行之前被自动的执行
若函数被设定为destructor属性，则该函数会在main（）函数执行之后或者exit（）被调用后被自动的执行。拥有此类属性的函数经常隐式的用在程序的初始化数据方面。

这两个属性还没有在面向对象C中实现。
```
__attribute__((constructor)) void before_main() {
   printf("--- %s\n", __func__);
}

__attribute__((destructor)) void after_main() {
   printf("--- %s\n", __func__);
}
  
int main(int argc, char **argv) {
   printf("--- %s\n", __func__);
   
   exit(0);
   
   printf("--- %s, exit ?\n", __func__);

   return 0;
}
```
执行结果为:
```
--- before_main
--- main
--- after_main
```
# 变量属性（Variable Attribute）