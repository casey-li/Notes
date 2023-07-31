**C++基础语法**

- [1、 在main执行之前和之后执行的代码可能是什么](#1-在main执行之前和之后执行的代码可能是什么)
- [2、结构体内存对齐问题？](#2结构体内存对齐问题)
- [3、指针和引用的区别](#3指针和引用的区别)
- [4、在传递函数参数时，什么时候该使用指针，什么时候该使用引用呢？](#4在传递函数参数时什么时候该使用指针什么时候该使用引用呢)
- [5、堆和栈的区别](#5堆和栈的区别)
- [6、你觉得堆快一点还是栈快一点？](#6你觉得堆快一点还是栈快一点)
- [7、区别以下指针类型？](#7区别以下指针类型)
- [8、new / delete 与 malloc / free 的异同](#8new--delete-与-malloc--free-的异同)
- [9、new 和 delete 是如何实现的？](#9new-和-delete-是如何实现的)
- [10、malloc和new的区别？](#10malloc和new的区别)
- [11、既然有了malloc/free，C++中为什么还需要new/delete呢？直接用malloc/free不好吗？](#11既然有了mallocfreec中为什么还需要newdelete呢直接用mallocfree不好吗)
- [12、被free回收的内存是立即返还给操作系统吗？](#12被free回收的内存是立即返还给操作系统吗)
- [13、宏定义和函数有何区别？](#13宏定义和函数有何区别)
- [14、宏定义和typedef区别？](#14宏定义和typedef区别)
- [15、变量声明和定义区别？](#15变量声明和定义区别)
- [16、strlen和sizeof区别？](#16strlen和sizeof区别)
- [17、常量指针和指针常量区别？](#17常量指针和指针常量区别)
- [18、a 和 \&a 有什么区别？](#18a-和-a-有什么区别)
- [19、C++和Python的区别](#19c和python的区别)
- [20、C++和C语言的区别](#20c和c语言的区别)
- [21、C++与Java的区别](#21c与java的区别)
- [22、C++中 struct 和 class 的区别](#22c中-struct-和-class-的区别)
- [23、define宏定义和const的区别](#23define宏定义和const的区别)
- [24、C++中const和static的作用](#24c中const和static的作用)
- [25、C++的顶层const和底层const](#25c的顶层const和底层const)
- [26、数组名和指针（这里为指向数组首元素的指针）区别？](#26数组名和指针这里为指向数组首元素的指针区别)
- [27、final 和 override 关键字](#27final-和-override-关键字)
- [28、拷贝初始化和直接初始化](#28拷贝初始化和直接初始化)
- [29、初始化和赋值的区别](#29初始化和赋值的区别)
- [30、extern"C"的用法](#30externc的用法)
- [31、野指针和悬空指针](#31野指针和悬空指针)
- [32、C和C++的类型安全](#32c和c的类型安全)
- [33、C++中的重载、重写（覆盖）和隐藏的区别](#33c中的重载重写覆盖和隐藏的区别)
- [34、C++有哪几种的构造函数](#34c有哪几种的构造函数)
- [35、浅拷贝和深拷贝的区别](#35浅拷贝和深拷贝的区别)
- [36、内联函数和宏定义的区别](#36内联函数和宏定义的区别)
- [37、public，protected 和 private 的访问和继承权限](#37publicprotected-和-private-的访问和继承权限)
- [38、如何用代码判断大小端存储？](#38如何用代码判断大小端存储)
- [39、volatile、mutable和explicit关键字的用法](#39volatilemutable和explicit关键字的用法)
- [40、什么情况下会调用拷贝构造函数](#40什么情况下会调用拷贝构造函数)
- [41、C++中有几种类型的new](#41c中有几种类型的new)
- [42、C++的异常处理的方法](#42c的异常处理的方法)
- [43、static的用法和作用？](#43static的用法和作用)
- [44、指针和const的用法](#44指针和const的用法)
- [45、形参与实参的区别？](#45形参与实参的区别)
- [46、值传递、指针传递、引用传递的区别和效率](#46值传递指针传递引用传递的区别和效率)
- [47、静态变量什么时候初始化](#47静态变量什么时候初始化)
- [48、const关键字的作用有哪些?](#48const关键字的作用有哪些)
- [49、什么是类的继承？](#49什么是类的继承)
- [50、从汇编层去解释一下引用](#50从汇编层去解释一下引用)
- [51、深拷贝与浅拷可以描述一下吗？](#51深拷贝与浅拷可以描述一下吗)
- [52、new和malloc的区别](#52new和malloc的区别)
- [53、delete p、delete\[\] p、allocator都有什么作用？](#53delete-pdelete-pallocator都有什么作用)
- [54、new 和 delete 的实现原理， delete 是如何知道释放内存的大小的？](#54new-和-delete-的实现原理-delete-是如何知道释放内存的大小的)
- [55、malloc申请的存储空间能用delete释放吗?](#55malloc申请的存储空间能用delete释放吗)
- [56、malloc与free的实现原理？](#56malloc与free的实现原理)
- [57、malloc、realloc、calloc的区别](#57mallocrealloccalloc的区别)
- [58、类成员初始化方式？构造函数的执行顺序 ？为什么用成员初始化列表会快一些？](#58类成员初始化方式构造函数的执行顺序-为什么用成员初始化列表会快一些)
- [59、有哪些情况必须用到成员列表初始化？作用是什么？](#59有哪些情况必须用到成员列表初始化作用是什么)
- [60、C++中新增了string，它与C语言中的 `char *` 有什么区别吗？它是如何实现的？](#60c中新增了string它与c语言中的-char--有什么区别吗它是如何实现的)
- [61、什么是内存泄露，如何检测与避免](#61什么是内存泄露如何检测与避免)
- [62、对象复用的了解，零拷贝的了解](#62对象复用的了解零拷贝的了解)
- [63、介绍面向对象的三大特性，并且举例说明](#63介绍面向对象的三大特性并且举例说明)
- [64、成员初始化列表的概念，为什么用它会快一些？](#64成员初始化列表的概念为什么用它会快一些)
- [65、C++的四种强制转换 `reinterpret_cast, const_cast, static_cast, dynamic_cast`](#65c的四种强制转换-reinterpret_cast-const_cast-static_cast-dynamic_cast)
- [66、C++函数调用的压栈过程](#66c函数调用的压栈过程)
- [67、写C++代码时有一类错误是 coredump ，很常见，你遇到过吗？怎么调试这个错误？](#67写c代码时有一类错误是-coredump-很常见你遇到过吗怎么调试这个错误)
- [68、说说移动构造函数](#68说说移动构造函数)
- [69、C++中将临时变量作为返回值时的处理过程](#69c中将临时变量作为返回值时的处理过程)
- [70、如何获得结构成员相对于结构开头的字节偏移量](#70如何获得结构成员相对于结构开头的字节偏移量)
- [71、静态类型和动态类型，静态绑定和动态绑定的介绍](#71静态类型和动态类型静态绑定和动态绑定的介绍)
- [72、引用是否能实现动态绑定，为什么可以实现？](#72引用是否能实现动态绑定为什么可以实现)
- [73、全局变量和局部变量有什么区别？](#73全局变量和局部变量有什么区别)
- [74、指针加减计算要注意什么？](#74指针加减计算要注意什么)
- [75、 怎样判断两个浮点数是否相等？](#75-怎样判断两个浮点数是否相等)
- [76、方法调用的原理（栈，汇编）](#76方法调用的原理栈汇编)
- [77、C++中的指针参数传递和引用参数传递有什么区别？底层原理你知道吗？](#77c中的指针参数传递和引用参数传递有什么区别底层原理你知道吗)
- [78、类如何实现只能静态分配和只能动态分配](#78类如何实现只能静态分配和只能动态分配)
- [79、如果想将某个类用作基类，为什么该类必须定义而非声明？](#79如果想将某个类用作基类为什么该类必须定义而非声明)
- [80、 继承机制中对象之间如何转换？指针和引用之间如何转换？](#80-继承机制中对象之间如何转换指针和引用之间如何转换)

**C++之基础语法**

## 1、 在main执行之前和之后执行的代码可能是什么

**main函数执行之前**，主要就是初始化系统相关资源：

*   设置栈指针
*   初始化静态`static`变量和`global`全局变量，即`.data`段的内容
*   将未初始化部分的全局变量赋初值：数值型`short`，`int`，`long`等为`0`，`bool`为`FALSE`，指针为`NULL`等等，即`.bss`段的内容
*   全局对象初始化，在`main`之前调用构造函数，这是可能会执行前的一些代码
*   将main函数的参数`argc`，`argv`等传递给`main`函数，然后才真正运行`main`函数
*   `__attribute__((constructor))`

**main函数执行之后**：

*   全局对象的析构函数会在main函数之后执行；
*   可以用 **`atexit`** 注册一个函数，它会在main 之后执行;
*   `__attribute__((destructor))`

**`attribute`**

`attribute` 是一个编译指令，可以指定在声明时的错误检查及高级优化的特性；可以设置函数属性（Function Attribute ）、变量属性（Variable Attribute ）和类型属性（Type Attribute ）

`__attribute__` 前后都有两个下划线，并且后面会紧跟一对原括弧，括弧里面是相应的参数 (语法格式为：`attribute ((attribute-list))`)

- constructor属性：函数被设定为constructor属性时，函数会在main 函数执行之前会被自动的执行
- destructor属性：函数被设定为destructor属性时，函数会在main 函数执行之后会被自动的执行

```c++
#include <stdio.h>
#include <stdlib.h>

__attribute__((constructor))void before() {
    printf("this is constructor\n");
}

__attribute__((destructor))void after() {
    printf("this is destructor\n");
}

int main() {
    printf("this is main\n");
    return 0;
}

// this is constructor
// this is main
// this is destructor

// 还可以设置优先级参数
static  __attribute__((constructor(101))) void before1(){
    printf("before1\n");
}
static  __attribute__((constructor(102))) void before2(){
    printf("before2\n");
}
```

**`atexit`**

进程终⽌的⽅式有8种，前5种为正常终⽌，后三种为异常终⽌：
1. 从 main 函数返回；
2. 调⽤ exit 函数；
3. 调⽤ _exit 或 _Exit；
4. 最后⼀个线程从启动例程返回；
5. 最后⼀个线程调⽤ pthread_exit；
6. 调⽤ abort 函数；
7. 接到⼀个信号并终⽌；
8. 最后⼀个线程对取消请求做出响应。

`exit()` 和 `_exit()` 以及 `_Exit()` 函数的本质区别是是否立即进入内核，`_exit()` 以及 `_Exit()` 函数都是在调用后立即进入内核，而不会执行一些清理处理，但是`exit()` 则会执行一些清理处理，这也是为什么会存在 `atexit()` 函数的原因

`atexit` 函数 (`int atexit (void (*)(void))`) 是一个特殊的函数，它是在正常程序退出时调用的函数，我们把他叫为登记函数，⼀个进程可以登记若⼲个函数（至少32个，依赖于编译器），这些函数由 `exit()` ⾃动调⽤，这些函数被称为终⽌处理函数，`atexit` 函数可以登记这些函数，`exit()` 调⽤终⽌处理函数的顺序和登记的顺序相反

`atexit` 函数的用途是可以按照规定的顺序摧毁全局变量（类）。例如有个log类，你在其它的全局类里也有可能调用到Log类写日志。所以log 类必须最后被析构 。假如没有规定析构顺序，那么程序在退出时将有可能首先析构log类，那么其它的全局类在此时将无法正确写日志。 把数据写回文件, 删除临时文件, 这才是真正有用的

```c++
#include<stdio.h>
#include<stdlib.h>

void func1() {
	printf("The process is done...\n");
}

void func2(){
	printf("Clean up the processing\n");
}

void func3() {
	printf("Exit sucessful..\n");
}

int main() {
	atexit(func1);
	atexit(func2);
	atexit(func3);
	exit(0);
}

// Exit sucessful..
// Clean up the processing
// The process is done...
```
-------------------

## 2、结构体内存对齐问题？

*   结构体内成员按照声明顺序存储，第一个成员地址和整个结构体地址相同
*   未特殊说明时，按结构体中size最大的成员对齐（若有double成员，按8字节对齐）

struct 对齐规则
1. 结构体中元素是按照定义顺序一个个放到内存中的，但并不是紧密排列的。从结构体存储的首地址开始，每一个元素放置到内存中时，它都会认为内存是以它自己的大小来划分的，因此元素放置的位置一定会在自己大小的整数倍上开始（以结构体变量首地址为0计算）(若指定了对齐方式, 则应位于 min(自己大小的整数倍, 对齐方式的整数倍), 若改为 1 的话全部挨着排列)
2. 在按照第一规则进行内存分配后，需要检查计算分配的内存大小是否为所有成员中大小最大的成员的大小的整数倍，若不是，则补齐为它的整数倍
3. 如果一个结构体 B 里嵌套另一个结构体 A ，则结构体 A 应从 offset 为 A 内部最大成员的整数倍的地方开始存储（struct B 里存有 struct A，A 里有 `char, int, double` 等成员，那 A 应该从8的整数倍开始存储），结构体A中的成员的对齐规则仍满足规则一、规则二

c++11以后引入两个关键字 `alignof` 与 `alignas`，其中`alignof`可以计算出类型的对齐方式，`alignas`可以指定结构体的对齐方式。

但是`alignas`在某些情况下是不能使用的，具体见下面的例子:
```c++
// alignas 生效的情况

struct Info {
    uint8_t a;   // 1
    uint16_t b;  // 2
    uint8_t c;   // 1
};

cout << sizeof(Info) << endl;   // 6  2 + 2 + 2
cout << alignof(Info) << endl;  // 2

struct alignas(4) Info2 {
    uint8_t a;
    uint16_t b;
    uint8_t c;
};

cout << sizeof(Info2) << endl;   // 8  4 + 4
cout << alignof(Info2) << endl;  // 4

 1 字节  |  1 字节填充  |  2 字节  |  1 字节
+--------+-------------+---------+---------+
|   a    |   填充字节   |    b    |    c    |
+--------+-------------+---------+---------+


// alignas 失效的情况, 即`alignas` 指定的对齐方式小于自然对齐的最小单位，就会被忽略

struct Info {
    uint8_t a;   // 1
    uint32_t b;  // 4
    uint8_t c;   // 1
};

cout << sizeof(Info) << endl;   // 12  4 + 4 + 4
cout << alignof(Info) << endl;  // 4

struct alignas(2) Info2 {
    uint8_t a;
    uint32_t b;
    uint8_t c;
};

cout << sizeof(Info2) << endl;   // 12  4 + 4 + 4
cout << alignof(Info2) << endl;  // 4
```

*   如果想使用单字节对齐的方式，使用`alignas`是无效的。应该使用`#pragma pack(push,1)`或者使用`__attribute__((packed))`。

```c++
// 根据编译器的类型来定义 ONEBYTE_ALIGN 宏和设置结构体的对齐方式
#if defined(__GNUC__) || defined(__GNUG__)
    #define ONEBYTE_ALIGN __attribute__((packed))
#elif defined(_MSC_VER)
    #define ONEBYTE_ALIGN
    #pragma pack(push,1)
#endif

struct Info {
    uint8_t a;
    uint32_t b;
    uint8_t c;
} ONEBYTE_ALIGN;

// 根据编译器类型取消对齐设置，恢复默认的对齐方式
#if defined(__GNUC__) || defined(__GNUG__)
    #undef ONEBYTE_ALIGN
#elif defined(_MSC_VER)
    #pragma pack(pop)
    #undef ONEBYTE_ALIGN
#endif

cout << sizeof(Info) << endl;   // 6 1 + 4 + 1
cout << alignof(Info) << endl;  // 1
```
 
*   确定结构体中每个元素大小可以通过下面这种方法:

```c++
#if defined(__GNUC__) || defined(__GNUG__)
    #define ONEBYTE_ALIGN __attribute__((packed))
#elif defined(_MSC_VER)
    #define ONEBYTE_ALIGN
    #pragma pack(push,1)
#endif

/**
* 0 1   3     6   8 9            15
* +-+---+-----+---+-+-------------+
* | |   |     |   | |             |
* |a| b |  c  | d |e|     pad     |
* | |   |     |   | |             |
* +-+---+-----+---+-+-------------+
*/

// 使用了位域(bit-fields)的方式来定义成员变量的大小。位域允许你指定一个成员变量所占用的位数，从而节省内存空间
struct Info {
    uint16_t a : 1;
    uint16_t b : 2;
    uint16_t c : 3;
    uint16_t d : 2;
    uint16_t e : 1;
    uint16_t pad : 7;
} ONEBYTE_ALIGN;

#if defined(__GNUC__) || defined(__GNUG__)
    #undef ONEBYTE_ALIGN
#elif defined(_MSC_VER)
    #pragma pack(pop)
    #undef ONEBYTE_ALIGN
#endif

cout << sizeof(Info) << endl;   // 2
cout << alignof(Info) << endl;  // 1
```        
这种处理方式是`alignas`处理不了的。

-----------------

## 3、指针和引用的区别

*   是否为变量？ 是否可以嵌套？ 是否可以为空？ 是否必须初始化？ 初始化后是否可以改变？ sizeof结果？ 传参是否影响实参（指针不是同一个变量，只不过指向的地址相同）？ 
*   引用本质是一个指针，同样会占4字节内存；指针是具体变量，需要占用存储空间（在编译器看来，`int &b = a` 等价于 `int *const b = &a`; `b = 20` 等价于 `*b = 20`，自动转换为指针和自动解引用）

---------------------------------------

## 4、在传递函数参数时，什么时候该使用指针，什么时候该使用引用呢？

*   需要返回函数内局部变量的内存的时候用指针。使用指针传参需要开辟内存，用完要记得释放指针，不然会内存泄漏。而返回局部变量的引用是没有意义的
*   对栈空间大小比较敏感（比如递归）的时候使用引用。使用引用传递不需要创建临时变量，开销要更小
*   类对象作为参数传递的时候使用引用，这是C++类对象传递的标准方式
    
---------------

## 5、堆和栈的区别

*   申请方式不同
    *   栈由系统自动分配
    *   堆是自己申请和释放的
*   申请大小限制不同。
    *   栈顶和栈底是之前预设好的，栈是向栈底扩展，大小固定，可以通过 `ulimit -a` 查看，由`ulimit -s` 修改
    *   堆向高地址扩展，是不连续的内存区域，大小可以灵活调整
*   申请效率不同
    *   栈由系统分配，速度快，不会有碎片。
    *   堆由程序员分配，速度慢，且会有碎片。

栈空间默认是4M, 堆区一般是 1G - 4G

| | 堆 | 栈 |
| :-- | -- | -- |
| **管理方式** | 堆中资源由程序员控制（容易产生memory leak）| 栈资源由编译器自动管理, 无需手工控制|
| **内存管理机制** | 系统有一个记录空闲内存地址的链表, 当系统收到程序申请时, 遍历该链表并寻找第一个空间大于申请空间的堆结点, 删除空闲结点链表中的该结点, 并将该结点空间分配给程序 (大多数系统会在这块内存空间首地址记录本次分配的大小, 这样delete才能正确释放本内存空间, 另外系统会将多余的部分重新放入空闲链表中) | 只要栈的剩余空间大于所申请空间, 系统为程序提供内存, 否则报异常提示栈溢出 (这一块理解一下链表和队列的区别, 不连续空间和连续空间的区别, 应该就比较好理解这两种机制的区别了) |
| **空间大小** | 堆是不连续的内存区域 (因为系统是用链表来存储空闲内存地址, 自然不是连续的), 堆大小受限于计算机系统中有效的虚拟内存 (32bit 系统理论上是4G), 所以堆的空间比较灵活, 比较大 | 栈是一块连续的内存区域, 大小是操作系统预定好的, windows下栈大小是2M (也有是1M, 在编译时确定, VC中可设置) |
| **碎片问题** | 对于堆, 频繁的 new/delete 会造成大量碎片, 使程序效率降低 | 对于栈, 它是有点类似于数据结构上的一个先进后出的栈, 进出一一对应, 不会产生碎片 (看到这里我突然明白了为什么面试官在问我堆和栈的区别之前先问了我栈和队列的区别) |
| **生长方向** | 堆向上, 向高地址方向增长 | 栈向下, 向低地址方向增长 |
| **分配方式** | 堆都是动态分配 (没有静态分配的堆) | 栈有静态分配和动态分配, 静态分配由编译器完成 (如局部变量分配), 动态分配由 alloca 函数分配, 但栈的动态分配的资源由编译器进行释放, 无需程序员实现 |
| **分配效率** | 堆由C/C++函数库提供, 机制很复杂; 所以堆的效率比栈低很多 | 栈是其系统提供的数据结构, 计算机在底层对栈提供支持, 分配专门寄存器存放栈地址, 栈操作有专门指令 |

---

## 6、你觉得堆快一点还是栈快一点？

**栈快**

- 操作系统会在底层对栈提供支持，会分配专门的寄存器存放栈的地址，栈的入栈出栈操作也十分简单，并且有专门的指令执行，所以栈的效率比较高也比较快
- 堆的操作是由C/C++函数库提供的，在分配堆内存的时候需要一定的算法寻找合适大小的内存。并且获取堆的内容需要两次访问，第一次访问指针，第二次根据指针保存的地址访问内存，因此堆比较慢

---

## 7、区别以下指针类型？

`int *p[10]` : 指针数组, 是一个数组变量 (大小为10, 每个元素都是指向int类型的指针)
`int (*p)[10]` : 数组指针, 是指针类型, 指向一个大小为 10 的 int 类型的数组
`int *p(int)` : 函数声明, 函数名是p, 参数和返回值都是 int 类型
`int (*p)(int)` : 函数指针, 指向一个参数和返回值都是 int 类型的函数

---

## 8、new / delete 与 malloc / free 的异同

**相同点**

*   都可用于内存的动态申请和释放

**不同点**

*   `new / delete` 是C++运算符, `malloc / free` 是C/C++语言标准库函数
*   `new` 自动计算要分配的空间大小, `malloc` 需要手动计算
*   `new` 是类型安全的, `malloc` 不是

```c++
int *p = new float[2]; //编译错误
int *p = (int*)malloc(2 * sizeof(double));//编译无错误
```

*   `new` 调用名为 `operator new` 的标准库函数分配足够空间并调用相关对象的构造函数, `delete` 对指针所指对象运行适当的析构函数, 然后调用名为 `operator delete` 的标准库函数释放该对象所用内存。`malloc / free`均没有相关调用
*   后者需要库文件支持，前者不用
*   `new` 封装了 `malloc`, 直接 `free` 不会报错, 但是这只是释放内存而不会析构对象

---

## 9、new 和 delete 是如何实现的？

**`new`**
- (1) 调用名为 `operator new` 的标准库函数, 分配足够大的内存以保存指定类型的一个对象
- (2) 调用该类型的一个构造函数, 用指定初始化构造对象
- (3) 返回指向构造后的的对象的指针

**`delete`**
- (1) 对指针指向的对象运行适当的析构函数
- (2) 调用名为 `operator delete` 的标准库函数释放该对象所用内存

注意 new[] 的结果应该用 delete[] 释放（为内置类型的话用 delete 释放不会报错，但若为自定义类型的话会报错）

![](https://img-blog.csdnimg.cn/20210619155058681.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0hVQUVSQlVTSEk1MjE=,size_16,color_FFFFFF,t_70)

[博客]()

对于自定义类型来说 `delete[]` 会把 `new[]` 所返回的指针向前偏移 4/8 个字节的地址返回给free,因此free就能正确的释放掉整片空间 (多出来的 4/8 个字节存的元素个数, 就是用来让 `delete[]` 知道调用多少次析构函数的)

若调用 `delete` 的话, 会先执行第一个对象的析构函数, 随后将 new[] 的地址传给 free 进行释放 (不是 malloc 申请的首地址), 因此 free 无法识别该地址, 程序崩溃

小结:

- `new[]` 在给自定义类型空间时, 会多分配 4/8个字节用来存储元素个数.通过 `*(int*)(ptr-1)` 就能看到或者直接查看内存也可以看到. 所以 `delete` 用此时 `new[]` 返回的指针来调用自己底层的 `free` 时, `free` 就出错了,程序就会崩溃.

- `new[]` 在给内置类型申请空间时,不会存储元素个数, 因为自定义类型 `delete[]` 时没有析构函数可调用(所以没必要存储), `free` 不会出错

---

## 10、malloc和new的区别？

*   `malloc` 和 `free` 是标准库函数, 支持覆盖; `new` 和 `delete` 是运算符, 支持重载
*   `malloc` 仅仅分配内存空间, `free` 仅仅回收空间，不具备调用构造函数和析构函数功能, 用 `malloc` 分配空间存储类的对象存在风险; `new` 和 `delete` 除了分配回收功能外, 还会调用构造函数和析构函数
*   `malloc` 和 `free` 返回的是 `void` 类型指针 (必须进行类型转换), `new` 和 `delete` 返回的是具体类型指针
    
---

## 11、既然有了malloc/free，C++中为什么还需要new/delete呢？直接用malloc/free不好吗？

*   `malloc/free` 和 `new/delete` 都是用来申请内存和回收内存的
*   在对非基本数据类型的对象使用的时候，对象创建的时候还需要执行构造函数，销毁的时候要执行析构函数。而 **`malloc/free` 是库函数，是已经编译的代码，所以不能把构造函数和析构函数的功能强加给 `malloc/free`**，所以 `new/delete` 是必不可少的

---

## 12、被free回收的内存是立即返还给操作系统吗？

不是的，被 `free` 回收的内存会首先被 `ptmalloc` (内存管理器) 使用双链表保存起来，当用户下一次申请内存的时候，会尝试从这些内存中寻找合适的返回。这样就避免了频繁的系统调用，占用过多的系统资源。同时 `ptmalloc` 也会尝试对小块内存进行合并，避免过多的内存碎片

---

## 13、宏定义和函数有何区别？

*   宏在预处理阶段完成替换，之后被替换的文本参与编译，相当于直接插入了代码，运行时不存在函数调用，执行起来更快；函数调用在运行时需要跳转到具体调用函数
*   宏定义属于在结构中插入代码，没有返回值；函数调用具有返回值
*   宏定义参数没有类型，不进行类型检查；函数参数具有类型，需要检查类型
*   宏定义不要在最后加分号 (容易出错)

宏定义若替换的是数学表达式注意加括号来避免执行顺序不一致; 若一行写不下的话可使用续行符 `\` 把该串分若干行来写

---

## 14、宏定义和typedef区别？

*   宏主要用于定义常量及书写复杂的内容；typedef主要用于定义类型别名
*   宏替换发生在编译阶段**之前**，属于文本插入替换；typedef是编译的一部分
*   宏不检查类型；typedef会检查数据类型
*   宏不是语句，不在在最后加分号；typedef是语句，要加分号标识结束
*   注意对指针的操作, `typedef char * p_char` 和 `#define p_char char *` 区别巨大。

```c++
#define p_char char*
typedef char* p_char2;

// 区别 1
unsigned p_char2 pc; // 报错 error: expected initializer before 'pc', 即编译器认为 p_char2 为变量名而非类型
unsigned p_char pc2; // 不报错, 编译前被替换为了 unsigned char* pc2, 正确

//区别 2
p_char2 pc1, pc2;
cout << typeid(pc1).name() << ", " << typeid(pc2).name(); // Pc, Pc, 都是指针
p_char pc3, pc4;
cout << typeid(pc3).name() << ", " << typeid(pc4).name(); // Pc, c, 因为编译前被替换为了 char *pc3, pc4; 前者为指针, 后者为 char
```

---

## 15、变量声明和定义区别？

- 声明是指在代码中提供标识符（变量、函数、类等）的存在和类型信息，告诉编译器标识符的名称和类型，但不为其分配内存或提供实际实现
- 定义是在声明的基础上提供标识符的实际实现或分配内存空间。对于变量，定义会分配内存空间；对于函数，定义会提供函数的实际实现
- 相同变量可以在多处声明（外部变量extern），但只能在一处定义。
    
```c++
// 声明变量和函数
extern int num;        // 声明一个整数变量num
int add(int a, int b); // 声明一个名为add的函数，接受两个整数参数并返回整数

// 定义变量和函数
int num = 42;          // 定义一个整数变量num并分配内存空间
int add(int a, int b)  // 定义函数add的实现
{
    return a + b;
}
```

---

## 16、strlen和sizeof区别？

*   `sizeof` 是运算符，并不是函数，结果在编译时得到而非运行中获得; `strlen` 是字符处理的库函数
*   `sizeof` 参数可以是任何数据的类型或者数据（`sizeof` **参数不退化**, 即数组在传递给 `sizeof` 运算符时不会退化为指针，得到的是整个数组的大小; 指针在传递给 `sizeof` 运算符时计算的是指针的大小而不是其指向对象的大小）, `strlen` 的参数只能是字符指针且结尾是`\0` 的字符串
*   因为 `sizeof` 值在编译时确定，所以不能用来得到动态分配（运行时分配）存储空间的大小。

```c++
int main(int argc, char const *argv[]){
    const char* str = "name";
    sizeof(str); // 取的是指针str的长度，是8 (64位编译环境)
    strlen(str); // 取的是这个字符串的长度，不包含结尾的 \0。大小是4
    return 0;
}
```

**（补充题）一个指针占多少字节？**

一个指针占内存的大小跟编译环境有关，而与机器的位数无关
在64位编译环境下指针的占用大小为8字节；而在32位环境下指针占用大小为4字节

---

## 17、常量指针和指针常量区别？

*   指针常量是一个指针，读成常量的指针，**指向只读变量**，也就是后面所指明的 `int const` 和 `const int`, 都是一个常量，可以写作 `int const *p` 或 `const int *p`
*   常量指针是一个不能给改变指向的指针。指针是个常量，必须初始化，一旦初始化完成，它的值（也就是存放在指针中的地址）就不能在改变了，即不能中途改变指向，如 `int *const p`

---

## 18、a 和 &a 有什么区别？

假设数组 `int a[10]; int (*p)[10] = &a;` 其中：

*   `a` 是数组名，是数组首元素地址，+1 表示地址值加上一个 int 类型的大小，如果 a 的值是 `0x00000001`, 加 1 操作后变为 `0x00000005`, `*(a + 1) = a[1]`
*   `&a` 是数组的指针, 其类型为 `int (*)[10]`，它加 1 时系统会认为是数组首地址加上整个数组的偏移（10 个 int 型变量）, 值为数组 a 尾元素后一个元素的地址
*   若 `(int *)p` ，此时输出 `*p` 时，其值为 `a[0]` 的值，因为被转为 `int *` 类型，解引用时按照 int 类型大小来读取值

---

## 19、C++和Python的区别

包括但不限于：
*   Python是一种脚本语言，是解释执行的; 而C++是编译语言，是需要编译后在特定平台运行的>python可以很方便的跨平台，但是效率没有C++高
*   Python使用缩进来区分不同的代码块，C++使用花括号来区分
*   C++中需要事先定义变量的类型，而Python不需要，Python的基本数据类型只有数字，布尔值，字符串，列表，元组等等
*   Python的库函数比C++的多，调用起来很方便

---

## 20、C++和C语言的区别

*   C++中 `new` 和 `delete` 是对**内存分配**的运算符，取代了C中的 `malloc` 和 `free`
*   标准C++中的**字符串类**取代了标准C函数库头文件中的字符数组处理函数（C中没有字符串类型）
*   C++中用来做控制态**输入输出**的 iostream 类库替代了标准C中的 stdio 函数库
*   C++中的 `try/catch/throw` **异常处理**机制取代了标准C中的 `setjmp()` 和 `longjmp()` 函数
*   在C++中，允许有相同的函数名，不过它们的参数类型不能完全相同，这样这些函数就可以相互区别开来。而这在C语言中是不允许的。也就是C++可以**重载**，C语言不允许
*   C++语言中，允许**变量定义语句**在程序中的任何地方，只要在是使用它之前就可以；而C语言中，必须要在函数开头部分。而且C++不允许重复定义变量，C语言也是做不到这一点的
*   在C++中，除了值和指针之外，新增了引用。**引用型变量**是其他变量的一个别名，我们可以认为他们只是名字不相同，其他都是相同的。
*   C++相对与C增加了一些**关键字**，如：`bool、using、dynamic_cast、namespace` 等等

---

## 21、C++与Java的区别

**语言特性**

*   Java语言给开发人员提供了更为简洁的语法；完全面向对象，由于 JVM 可以安装到任何的操作系统上，所以说它的可移植性强
*   Java语言中没有**指针**的概念，引入了真正的**数组**。不同于C++中利用指针实现的“伪数组”，Java引入了真正的数组，同时将容易造成麻烦的指针从语言中去掉，这将有利于防止在C++程序中常见的因为数组操作越界等指针操作而对系统数据进行非法读写带来的不安全问题
*   C++也可以在其他系统运行，但是需要不同的**编码**（这一点不如Java，只编写一次代码，到处运行），例如对一个数字，在windows下是大端存储，在unix中则为小端存储。Java程序一般都是生成字节码，在 JVM 里面运行得到结果
*   Java用**接口**(Interface)技术取代C++程序中的**抽象类**。接口与抽象类有同样的功能，但是省却了在实现和维护上的复杂性
    
**垃圾回收**

*   C++用析构函数回收垃圾，写C和C++程序时一定要注意内存的申请和释放
*   Java语言不使用指针，内存的分配和回收都是自动进行的，程序员无须考虑内存碎片的问题

**应用场景**

*   Java在桌面程序上不如C++实用，C++可以直接编译成exe文件，指针是c++的优势，可以直接对内存的操作，但同时具有危险性 。（操作内存的确是一项非常危险的事情，一旦指针指向的位置发生错误，或者误删除了内存中某个地址单元存放的重要数据，后果是可想而知的）
*   Java在Web 应用上具有C++ 无可比拟的优势，具有丰富多样的框架
*   对于底层程序的编程以及控制方面的编程，C++很灵活，因为有句柄的存在

---

## 22、C++中 struct 和 class 的区别

**相同点**

*   两者都拥有成员函数、公有和私有部分
*   任何可以使用class完成的工作，同样可以使用struct完成

**不同点**

*   两者中如果不对成员不指定公私有，struct默认是公有的，class则默认是私有的
*   class默认是private继承， 而struct默认是public继承
    

**引申**：C++和C的struct区别

*   C语言中：struct是用户自定义**数据类型**（UDT）；C++中struct是抽象数据类型（ADT），支持成员函数的定义，（C++中的struct能继承，能实现多态）
*   C中struct是没有权限的设置的，且struct中只能是一些变量的集合体，可以封装数据却不可以隐藏数据，而且成员**不可以是函数**
*   C++中，struct增加了**访问权限**，且可以和类一样有成员函数，成员默认访问说明符为public（为了与C兼容）
*   struct作为类的一种特例是用来自定义数据结构的。一个结构标记声明后，在C中必须在结构标记前加上struct，才能做结构类型名（除：typedef struct class{};）;C++中结构体标记（结构体名）可以直接作为结构体类型名使用，此外结构体struct在C++中被当作类的一种特例
    
---

## 23、define宏定义和const的区别

**编译阶段**

*   define是在编译的**预处理**阶段起作用，而const是在编译、运行的时候起作用

**安全性**

*   define只做替换，不做类型检查和计算，也不求解，容易产生错误，一般最好加上一个大括号包含住全部的内容，要不然很容易出错
*   const常量有数据类型，编译器可以对其进行类型安全检查

**内存占用**

*   define只是将宏名称进行替换，在内存中会产生多份相同的备份。const在程序运行中只有一份备份，且可以执行常量折叠，能将复杂的的表达式计算出结果放入常量表
*   宏定义的数据没有分配内存空间，只是插入替换掉；const定义的变量只是值不能改变，但要分配内存空间
    
---

## 24、C++中const和static的作用

**static**

*   不考虑类的情况
    *   **隐藏**。所有不加static的全局变量和函数具有全局可见性，可以在其他文件中使用，加了之后只能在该文件所在的编译模块中使用
    *   **默认初始化为0**，包括未初始化的全局静态变量与局部静态变量，都存在全局未初始化区
    *   静态变量在函数内定义，始终存在，且只进行一次初始化，具有记忆性，其**作用范围与局部变量相同**，函数退出后仍然存在，但不能使用
*   考虑类的情况
    *   static成员变量：只与类关联，不与类的对象关联。**定义时要分配空间**，不能在类声明中初始化，**必须在类定义体外部初始化**，初始化时不需要标示为static；可以被非static成员函数任意访问
    *   static成员函数：**不具有this指针**，无法访问类对象的非static成员变量和非static成员函数；**不能被声明为const、虚函数和 volatile**；可以被非static成员函数任意访问

> volatile 用于声明变量是易变的, 它告诉编译器不要对该变量进行优化, 可以确保每次访问都是直接从内存中而不是缓存中读取

**const**

*   不考虑类的情况
    *   const常量在定义时**必须初始化**，之后无法更改
    *   const形参可以接收const和非const类型的实参，如在 `void fun(const int& i)`{ //...} 中, i 可以是 `int` 型或者 `const int` 型
*   考虑类的情况
    *   const成员变量：不能在类定义外部初始化，只能通过**构造函数初始化列表进行初始化**，并且必须有构造函数；不同类对其const数据成员的值可以不同，所以**不能在类中声明时初始化**
    *   const成员函数：**const对象不可以调用非const成员函数**；非const对象都可以调用；不可以改变非 `mutable`（用该关键字声明的变量可以在const成员函数中被修改）数据的值

补充一点const相关：const修饰变量是也与static有一样的**隐藏**作用。只能在该文件中使用，其他文件不可以引用声明使用。 因此在头文件中声明const变量是没问题的，因为即使被多个文件包含，链接性都是内部的，不会出现符号冲突

---

## 25、C++的顶层const和底层const

**概念区分**

*   **顶层**const：指的是const修饰的变量**本身**是一个常量，无法修改，指的是指针，就是 * 号的右边
*   **底层**const：指的是const修饰的变量**所指向的对象**是一个常量，指的是所指变量，就是 * 号的左边

只有指针和引用有底层 const

```c++
int a = 10; int* const b1 = &a;        // 顶层const，b1本身是一个常量
const int* b2 = &a;         // 底层const，b2本身可变，所指的对象是常量
const int b3 = 20; 		    // 顶层const，b3是常量不可变
const int* const b4 = &a;   // 前一个const为底层，后一个为顶层，b4不可变
const int& b5 = a;		    // 用于声明引用变量，都是底层const
```

**区分作用**

*   执行对象拷贝时有限制，常量的底层 const 不能赋值给非常量的底层 const
*   使用命名的强制类型转换函数 `const_cast` 时，只能改变运算对象的**底层 const**

---

## 26、数组名和指针（这里为指向数组首元素的指针）区别？

*   二者均可通过增减偏移量来访问数组中的元素
*   数组名不是真正意义上的指针，可以理解为**常量指针**，所以**数组名没有自增、自减等操作**
*   **当数组名当做形参传递给调用函数后，就失去了原有特性，退化成一般指针，多了自增、自减操作，但sizeof运算符不能再得到原数组的大小了**
    
---

## 27、final 和 override 关键字

- **override**

当在父类中使用了虚函数时候，你可能需要在某个子类中对这个虚函数进行重写，以下方法都可以：

```c++
class A {
    virtual void foo();
}
class B : public A {
    void foo(); //OK
    virtual void foo(); // OK
    void foo() override; //OK
}
```
    
如果不使用 `override`, 不小心将 `foo()` 写成了 `f00()` 的话编译器并不会报错, 因为它并不知道你的目的是重写虚函数, 而是把它当成了新的函数。如果这个虚函数很重要的话，那就会对整个程序不利。所以, **`override` 可以指定子类的这个虚函数是重写父类的函数**, 如果不小心打错了名字的话就无法通过编译器

```c++
class A {
    virtual void foo();
};
class B : public A {
    virtual void f00(); //OK，这个函数是B新增的，不是继承的
    virtual void f0o() override; //Error, 加了override之后，这个函数一定是继承自A的，A找不到就报错
};
```

- **final**

**当不希望某个类被继承, 或不希望某个虚函数被重写, 可以在类名和虚函数后添加final关键字**，添加final关键字后被继承或重写，编译器会报错

```c++
class Base {
    virtual void foo();
};
    
class A : public Base {
    void foo() final; // foo 被override并且是最后一个override，在其子类中不可以重写
};

class B final : A { // 指明B是不可以被继承的
    void foo() override; // Error: 在A中已经被final了
};
    
class C : B { // Error: B is final
};
```

---

## 28、拷贝初始化和直接初始化

*   当用于**类类型对象**时，初始化的拷贝形式 (=) 和直接形式有所不同：**直接初始化直接调用与实参匹配的构造函数，拷贝初始化总是调用拷贝构造函数**。拷贝初始化首先使用指定构造函数创建一个临时对象，然后用拷贝构造函数将那个临时对象拷贝到正在创建的对象

```c++
string str1("I am a string"); // 语句1 直接初始化
string str2(str1); // 语句2 直接初始化，str1是已经存在的对象，直接调用拷贝构造函数对str2进行初始化
string str3 = "I am a string"; // 语句3 拷贝初始化，先为字符串 "I am a string" 创建临时对象，再把临时对象作为参数，使用拷贝构造函数构造str3
string str4 = str1; // 语句4 拷贝初始化，这里相当于隐式调用拷贝构造函数，而不是调用赋值运算符函数
```

*   **为了提高效率，允许编译器跳过创建临时对象这一步**，直接调用构造函数构造要创建的对象，这样就完全等价于**直接初始化了**（语句1和语句3等价），但是需要辨别两种情况
    *   当拷贝构造函数为 `private` 时：语句3和语句4在编译时会报错
    *   使用 `explicit` 修饰构造函数时：如果构造函数存在隐式转换，编译时会报错

---

## 29、初始化和赋值的区别

*   对于简单类型来说，初始化和赋值没什么区别
*   对于类和复杂数据类型来说，这两者的区别就大了，举例如下：

```c++
class A {
public:
    int num1;
    int num2;
public:
    A(int a = 0, int b = 0): num1(a), num2(b){};
    // 拷贝构造
    A(const A& a) {
        this->num1 = a.num1 + 2;
        this->num2 = a.num2 + 2;
    };
    // 重载 = 号操作符函数
    A& operator=(const A& a) {
        num1 = a.num1 + 1;
        num2 = a.num2 + 1;
        return *this;
    };
};

int main() {
    A a(1,1); // num1 = 1, nums2 = 1
    A a1 = a; // 拷贝初始化操作，调用拷贝构造函数, num1 = 3, nums2 = 3
    A b;
    b = a; // 赋值操作, 调用赋值运算符重载函数, num1 = 2，num2 = 2
    return 0;
}
```

---

## 30、extern"C"的用法

为了能够**正确的在C++代码中调用C语言**的代码：在程序中加上 `extern "C"` 后，相当于告诉编译器这部分代码是C语言写的，因此要**按照C语言进行编译，而不是C++**。使用extern "C" 的情况

- C++代码中调用C语言代码；
- 在C++中的头文件中使用；
- 在多个人协同开发时，可能有人擅长C语言，而有人擅长C++；

```c++
#ifndef __MY_HANDLE_H__
#define __MY_HANDLE_H__

extern "C"{
    typedef unsigned int result_t;
    typedef void* my_handle_t;
    
    my_handle_t create_handle(const char* name);
    result_t operate_on_handle(my_handle_t handle);
    void close_handle(my_handle_t handle);
}
```

在C语言的头文件中，对其外部函数只能指定为 `extern` 类型，**C语言中不支持extern "C"声明**，在 `.c` 文件中包含了 `extern "C"` 时会出现编译语法错误。所以**使用 extern "C" 必须全部都放在于cpp程序相关文件或其头文件中**

- C++ 调用 C 函数

```c++
//xx.h
extern int add(...)

//xx.c
int add(){
    
}

//xx.cpp
extern "C" {
    #include "xx.h"
}
```

- C 调用 C++ 函数
 
```c++
//xx.h
extern "C"{
    int add();
}

//xx.cpp
int add(){    
}

//xx.c
extern int add();
```

c++ 中 `extern int add(...)` 写在 `XX.h` 中, `extern "C"` 写在 `XX.cpp` 中并将 `XX.c` 头文件导入
c 中 `extern int add(...)` 写在 `XX.c` 中, `extern "C"` 写在头文件中并声明函数
    
## 31、野指针和悬空指针

都是是指向无效内存区域 (这里的无效指的是"不安全不可控") 的指针，访问行为将会导致未定义行为

*   野指针

野指针，指的是没有被初始化过的指针

```c++
int main(void) { 
    int* p;     // 未初始化
    cout<< *p << endl; // 未初始化就被使用
    return 0;
}
```
因此，为了防止出错，对于指针初始化时都是赋值为 `nullptr`，这样在使用时编译器就不会直接报错，产生非法内存访问
    
*   悬空指针  

悬空指针，指针最初指向的内存已经被释放了的一种指针

```c++
int main(void) { 
    int * p = nullptr;
    int* p2 = new int;
    p = p2;
    delete p2;
}
```

此时 p 和 p2 就是悬空指针，指向的内存已经被释放。继续使用这两个指针，行为不可预料。需要设置为`p = p2 = nullptr`
避免野指针比较简单，但悬空指针比较麻烦。c++引入了智能指针，**C++智能指针的本质就是避免悬空指针的产生**

**产生原因及解决办法：**

野指针：指针变量未及时初始化 => 定义指针变量及时初始化或者置空

悬空指针：指针 `free` 或 `delete` 之后没有及时置空 => 释放操作后立即置空

---

## 32、C和C++的类型安全

- **什么是类型安全？**

**类型安全很大程度上可以等价于内存安全，类型安全的代码不会试图访问自己没被授权的内存区域**。“类型安全”常被用来形容编程语言，其根据在于该门编程语言是否提供保障类型安全的机制；有的时候也用“类型安全”形容某个程序，判别的标准在于该程序是否隐含类型错误

类型安全的编程语言与类型安全的程序之间，没有必然联系。好的程序员可以使用类型不那么安全的语言写出类型相当安全的程序，相反的，差一点儿的程序员可能使用类型相当安全的语言写出类型不太安全的程序。绝对类型安全的编程语言暂时还没有

- **1、C的类型安全**

C只在局部上下文中表现出类型安全，比如试图从一种结构体的指针转换成另一种结构体的指针时，编译器将会报告错误，除非使用显式类型转换。然而，C中相当多的操作是不安全的。以下是两个十分常见的例子：

*   printf 格式输出

```c++
#include <stdio.h>

int main() {
    printf("整形输出：%d\n", 10);   // 整形输出：10
    printf("浮点型输出：%f\n", 10); // 浮点型输出：0.000000
    return 0;
}
```

上述代码中，使用 `%d` 控制整型数字的输出，没有问题，但是改成 `%f` 时，明显输出错误，再改成 `%s` 时，运行直接报 `segmentation fault` 错误

*   malloc 函数的返回值

`malloc` 是C中进行内存分配的函数，它的返回类型是 `void*` 即空类型指针，常常有这样的用法 `char *pStr = (char*)malloc(100 * sizeof(char))`, 这里明显做了显式的类型转换

类型匹配尚且没有问题，但是一旦出现 `int *pInt = (int*)malloc(100*sizeof(char))` 就很可能带来一些问题，而这样的转换C并不会提示错误

- **2、C++的类型安全**

如果C++使用得当，它将远比C更有类型安全性。相比于C语言，C++提供了一些新的机制保障类型安全：

*   操作符 `new` 返回的指针类型严格与对象匹配，而不是 `void*`
*   C中很多以 `void*` 为参数的函数可以改写为 C++ **模板函数**，而模板是支持类型检查的
*   **引入 const 关键字代替 #define constants**, 它是有类型、有作用域的，而 `#define constants` 只是简单的文本替换
*   一些 #define 宏可被改写为 **inline 函数**，结合函数的重载，可在类型安全的前提下支持多种类型，当然改写为模板也能保证类型安全
*   C++提供了 `dynamic_cast` 关键字, 使得转换过程更加安全，因为 `dynamic_cast` 比 `static_cast` 涉及更多具体的类型检查。
    
例1：使用 `void *` 进行类型转换

```c++
#include <iostream>
using namespace std;

int main() {
    int i = 5;
    void *pInt = &i;
    double d = *(double *)pInt;
    cout << "转换后输出：" << d << endl; // 转换后输出：3.76809e-308
    return 0;
}
```

​例2：不同类型指针之间转换

```c++
#include<iostream>
using namespace std;
    
class Parent {};

class Child1 : public Parent {
public:
    int i;
    Child1(int e) : i(e) {}
};

class Child2 : public Parent {
public:
    double d;
    Child2(double e) : d(e) {}
};

int main() {
    Child1 c1(5);
    Child2 c2(4.1);
    Parent *pp;
    Child1 *pc1;

    pp = &c1;
    pc1 = (Child1 *)pp;     // 类型向下转换 强制转换，由于类型仍然为Child1*，不造成错误
    cout << pc1->i << endl; // 输出：5

    pp = &c2;
    pc1 = (Child1 *)pp;     // 强制转换，且类型发生变化，将造成错误
    cout << pc1->i << endl; // 输出：1717986918
    return 0;
}
```

上面两个例子之所以引起类型不安全的问题，是因为程序员使用不得当。第一个例子用到了空类型指针 `void*`, 第二个例子则是在两个类型指针之间进行强制转换。因此，想保证程序的类型安全性，应**尽量避免使用空类型指针 `void*`，尽量不对两种类型指针做强制转换**

---

## 33、C++中的重载、重写（覆盖）和隐藏的区别

- 重载（overload）

重载是指在同一范围定义中的同名成员函数才存在重载关系。主要特点是**函数名相同，参数类型和数目有所不同**，不能出现参数个数和类型均相同，**返回值不同不能用来区分不同的函数**。重载和函数成员是否是虚函数无关

```c++
class A{
    ...
    virtual int fun();
    void fun(int);
    void fun(double, double);
    static int fun(char);
    ...
}
```

- 重写（覆盖）（override）

重写指的是在派生类中覆盖基类中的同名函数，**重写就是重写函数体, 要求基类函数必须是虚函数**且：

*   与基类的虚函数有相同的参数个数
*   与基类的虚函数有相同的参数类型
*   与基类的虚函数有相同的返回值类型

```c++
class A {
public:
    virtual int fun(int a){}
}

class B : public A {
public:
    //重写, 一般加override可以确保是重写父类的函数
    virtual int fun(int a) override{}
}
```

**重载与重写的区别：**

*   重写是父类和子类之间的垂直关系，重载是不同函数之间的水平关系
*   重写要求参数列表相同，重载则要求参数列表不同，返回值不要求
*   重写关系中，调用方法根据对象类型决定，重载根据调用时实参表与形参表的对应关系来选择函数体

- 隐藏（hide）

隐藏指的是某些情况下，**派生类中的函数屏蔽了基类中的同名函数**，包括以下情况：

1. 两个函数**参数相同**，但是基类函数不是虚函数。**隐藏和重写的区别在于基类函数是否是虚函数**, 若想调用被隐藏的基类函数, 使用作用域运算符 (Son.Base::func())

```c++

class A{
public:
    void fun(int a){
        cout << "A中的fun函数" << endl;
    }
};


class B : public A{
public:
    //隐藏父类的fun函数
    void fun(int a){
        cout << "B中的fun函数" << endl;
    }
};

int main(){
    B b;
    b.fun(2); //调用的是B中的fun函数
    b.A::fun(2); //调用A中fun函数
    return 0;
}
```

2. 两个函数**参数不同**，无论基类函数是不是虚函数，都会被隐藏。**和重载的区别在于两个函数不在同一个类中**

```c++
class A {
public:
    virtual void fun(int a){
        cout << "A中的fun函数" << endl;
    }
};

class B : public A{
public:
    //隐藏父类的fun函数
    virtual void fun(char* a){
        cout << "A中的fun函数" << endl;
    }
};

int main(){
    B b;
    b.fun(2); //报错，调用的是B中的fun函数，参数类型不对
    b.A::fun(2); //调用A中fun函数
    return 0;
}
```

补充：只有重写 (覆盖) 的函数支持多态; 若为隐藏, 那么即使基类的指针指向派生类也只能调用基类中被隐藏的函数

```c++
class A {
public:
    virtual void fun(int a) { // 虚函数
        cout << "This is A fun " << a << endl;
    }  
    void add(int a, int b) {
        cout << "This is A add " << a + b << endl;
    }
};

class B: public A {
public:
    void fun(int a) override {  // 覆盖
        cout << "this is B fun " << a << endl;
    }
    void add(int a) {   // 隐藏
        cout << "This is B add " << a + a << endl;
    }
};

int main() {
    // 基类指针指向派生类对象时，基类指针可以直接调用到派生类的覆盖函数，也可以通过 :: 调用到基类被覆盖
    // 的虚函数；而基类指针只能调用基类的被隐藏函数，无法识别派生类中的隐藏函数。

    A *p = new B();
    p->fun(1);      // 调用子类 fun 覆盖函数
    p->A::fun(1);   // 调用父类 fun
    p->add(1, 2);
    // p->add(1);      // 错误，识别的是 A 类中的 add 函数，参数不匹配
    // p->B::add(1);   // 错误，无法识别子类 add 函数
    return 0;
}
```

---

## 34、C++有哪几种的构造函数

*   默认构造函数
*   初始化构造函数 (有参数)
*   拷贝构造函数
*   移动构造函数 (move 和 右值引用)
*   委托构造函数 (输出两条语句)
*   转换构造函数

```c++
class MyClass {
public:
    // 默认构造函数
    MyClass() { 
        cout << "Default constructor called." << endl;
    }

    // 初始化构造函数
    MyClass(const string& value) : data(value) { 
        cout << "Initialization constructor called. Value: " << data << endl;
    }

    // 拷贝构造函数
    MyClass(const MyClass& other) : data(other.data) { 
        cout << "Copy constructor called. Value: " << data << endl;
    }

    // 移动构造函数
    MyClass(MyClass&& other) noexcept : data(move(other.data)) { 
        cout << "Move constructor called. Value: " << data << endl;
    }

    // 委托构造函数
    MyClass(int number) : MyClass("Number: " + to_string(number)) {
        cout << "Delegating constructor" << endl;
    }

    // 转换构造函数
    explicit MyClass(double number) : data_(to_string(number)) {
        cout << "Conversion constructor" << endl;
    }

    // 拷贝赋值
    MyClass& operator=(const MyClass& other) { 
        data = other.data;
        cout << "Copy assignment operator called. Value: " << data << endl;
        return *this;
    }

    // 移动赋值
    MyClass& operator=(MyClass&& other) noexcept { 
        data = move(other.data);
        cout << "Move assignment operator called. Value: " << data << endl;
        return *this;
    }
private:
    string data;
};

int main() {
    MyClass obj1;           // 默认构造函数
    MyClass obj2("Hello");  // 初始化构造函数
    MyClass obj3 = obj2;    // 拷贝构造函数
    MyClass obj4 = move(obj2);  // 移动构造函数
    MyClass obj5 = 42;          // 委托构造函数, 会先输出初始化构造再输出委托构造
    MyClass obj6 = 3.14;        // 调用结果同上，调用的委托构造函数，因为转换构造函数有 explicit 关键字，data为 Number: 3
    MyClass obj7(3.14);         // 转换构造函数，data为 Number: 3.14
    obj7 = obj5;                // 拷贝赋值运算符
    MyClass obj8;
    obj8 = move(obj6);          // 移动赋值运算符

    return 0;
}

```

*   默认构造函数和初始化构造函数在定义类的对象，完成对象的初始化工作
*   复制构造函数用于复制本类的对象
*   转换构造函数用于将其他类型的变量，隐式转换为本类对象

---

## 35、浅拷贝和深拷贝的区别

**浅拷贝**

浅拷贝**只是拷贝一个指针，并没有新开辟一个地址**，拷贝的指针和原来的指针指向同一块地址，如果原来的指针所指向的资源释放了，那么再释放浅拷贝的指针的资源就会出现错误

**深拷贝**

深拷贝不仅拷贝值，还**开辟出一块新的空间用来存放新的值**，即使原先的对象被析构掉，释放内存了也不会影响到深拷贝得到的值。**在实现拷贝赋值的时候，如果有指针变量的话是需要自己实现深拷贝的**

```c++
#include <iostream>  
#include <string.h>
using namespace std;
    
class Student
{
private:
    int num;
    char *name;
public:
    Student() {
        name = new char(20);
    };

    ~Student() {
        cout << "~Student " << &name << endl;
        delete name;
        name = NULL;
    };

    //拷贝构造函数
    Student(const Student &s) {
        //浅拷贝，当对象的name和传入对象的name指向相同的地址
        name = s.name;
        //深拷贝
        //name = new char(20);
        //memcpy(name, s.name, strlen(s.name));
        cout << "copy Student" << endl;
    };
};
    
int main()
{
    // 花括号让s1和s2变成局部对象，方便测试
    {
        Student s1;
        Student s2(s1);// 复制对象
    }
    system("pause");
    return 0;
}
//浅拷贝执行结果：
//copy Student
//~Student 0x7fffed0c3ec0
//~Student 0x7fffed0c3ed0
//*** Error in `/tmp/815453382/a.out': double free or corruption (fasttop): 0x0000000001c82c20 ***

//深拷贝执行结果：
//copy Student
//~Student 0x7fffebca9fb0
//~Student 0x7fffebca9fc0
```  

浅拷贝在对象的拷贝创建时存在风险，会对同一个资源释放两次；深拷贝得到的两个对象之间没有任何关系，各自成员地址不同

---

## 36、内联函数和宏定义的区别

*   在使用时，宏只做简单字符串替换（编译前）。而内联函数可以进行参数类型检查（编译时）, 且具有返回值
*   内联函数在编译时直接将函数代码嵌入到目标代码中，省去函数调用的开销来提高执行效率，并且**进行参数类型检查，具有返回值，可以实现重载**
*   宏定义时要注意书写（参数要括起来）否则容易出现歧义，内联函数不会产生歧义
*   内联函数有类型检测、语法判断等功能，而宏没有

**内联函数适用场景:**

*   使用宏定义的地方都可以使用 inline 函数
*   作为类成员接口函数来读写类的私有成员或者保护成员，会提高效率

---

## 37、public，protected 和 private 的访问和继承权限

*   `public` 的变量和函数在**类的内部外部**都可以访问
*   `protected` 的变量和函数只能在**类的内部和其派生类**中访问
*   `private` 修饰的元素只能在**类内**访问
    

（一）访问权限

**派生类可以继承基类中除了构造/析构、赋值运算符重载函数之外的成员，但是这些成员的访问属性在派生过程中也是可以调整的**，三种派生方式的访问权限如下表所示：注意外部访问并不是真正的外部访问，而是在通过派生类的对象对基类成员的访问

![](http://oss.interviewguide.cn/img/202205212341241.png)

派生类对基类成员的访问形象有如下两种：

*   内部访问：由派生类中新增的成员函数对从基类继承来的成员的访问
*   **外部访问**：在派生类外部，通过派生类的对象对从基类继承来的成员的访问

**派生类对基类成员的访问权限是取两点 (一、继承方式；二、基类成员在基类中的访问权限) 中的更小的访问范围（除了 private 的继承方式遇到 private 成员是不可见外）**

（二）继承权限

**public继承**

公有继承的特点是基类的**公有成员和保护成员**作为派生类的成员时，都**保持原有的状态**，而基类的**私有成员任然是私有的，不能被这个派生类的子类所访问**

**protected继承**

保护继承的特点是基类的所有**公有成员和保护成员都成为派生类的保护成员**，并且**只能被它的派生类成员函数或友元函数访问**，基类的私有成员仍然是私有的，访问规则如下表

![](http://oss.interviewguide.cn/img/202205212341074.png)

**private继承**

私有继承的特点是基类的**所有公有成员和保护成员都成为派生类的私有成员**，并不被它的派生类的子类所访问，基类的成员只能由自己派生类访问，无法再往下继承，访问规则如下表

![](http://oss.interviewguide.cn/img/202205212341430.png)

**总结**

一、访问权限

| 访问权限 | 外部 | 派生类 | 内部 |
| --- | --- | --- | --- |
| public | ✔ | ✔ | ✔ |
| protected | ❌ | ✔ | ✔ |
| private | ❌ | ❌ | ✔ |

二、继承权限

1.  派生类继承自基类的成员权限有四种状态：**public、protected、private、不可见**
2.  派生类对基类成员的访问权限取决于两点：一、继承方式；二、基类成员在基类中的访问权限
3.  派生类对基类成员的访问权限是取以上两点中的更小的访问范围（除了 private 的继承方式遇到 private 成员是不可见外）

*   public 继承 + private 成员 => private
*   private 继承 + protected 成员 => private
*   private 继承 + private 成员 => 不可见

---

## 38、如何用代码判断大小端存储？

大端存储：字数据的高字节存储在低地址中

小端存储：字数据的低字节存储在低地址中

**在Socket编程中，往往需要将操作系统所用的小端存储的IP地址转换为大端存储，这样才能进行网络传输**

内存从左往右自低向高增长, 二进制从右往左自低向高增长

例如：32bit的数字 0x12345678

小端模式中的存储方式为：

![](http://oss.interviewguide.cn/img/202205071832785.png)

大端模式中的存储方式为：

![](http://oss.interviewguide.cn/img/202205071832707.png)

了解了大小端存储的方式，如何在代码中进行判断呢？

**方式一：使用强制类型转换**

```c++
#include <iostream>
using namespace std;

int main()
{
    int a = 0x1234;
    //由于int和char的长度不同，借助int型转换成char型，只会留下低地址的部分
    char c = (char)(a);
    if (c == 0x12)
        cout << "big endian" << endl;
    else if(c == 0x34)
        cout << "little endian" << endl;
}
```

**方式二：巧用union联合体**

```c++
#include <iostream>
using namespace std;
//union联合体的重叠式存储，endian联合体占用内存的空间为每个成员字节长度的最大值
union endian {
    int a;
    char ch;
};

int main()
{
    endian value;
    value.a = 0x1234;
    //a和ch共用4字节的内存空间
    if (value.ch == 0x12)
        cout << "big endian"<<endl;
    else if (value.ch == 0x34)
        cout << "little endian"<<endl;
}
```
---

## 39、volatile、mutable和explicit关键字的用法

- **volatile**

volatile 关键字是一种类型修饰符，**用它声明的类型变量表示可以被某些编译器未知的因素更改**，比如：操作系统、硬件或者其它线程等。遇到这个关键字声明的变量，编译器对访问该变量的代码就不再进行优化，从而可以提供对特殊地址的稳定访问

当要求使用 volatile 声明的变量的值的时候，**系统总是重新从它所在的内存读取数据**，即使它前面的指令刚刚从该处读取过数据。

**volatile定义变量的值是易变的，每次用到这个变量的值的时候都要去重新读取这个变量的值，而不是读寄存器内的备份。多线程中被几个任务共享的变量需要定义为volatile类型。**

**volatile 指针**

volatile 指针和 const 修饰词类似，const 有常量指针和指针常量的说法，volatile 也有相应的概念

```c++
// 修饰指针指向的对象、数据是 const 或 volatile 的
const char* cpch;
volatile char* vpch;

// 修饰指针自身的值是 const 或 volatile 的
char* const pchc;
char* volatile pchv;
```

注意：
*   可以把一个非 `volatile int` 赋给 `volatile int` ，但是不能把 `volatile int` 赋给一个 非 `volatile int`
*   除了基本类型外，对用户定义类型也可以用 volatile 进行修饰
*   如果一个类被声明为带有 `volatile` 修饰符，那么对该类的对象的访问会遵循 `volatile` 的规则。这也意味着只能访问该类的接口中由实现者明确声明为 `volatile` 的成员函数和成员变量
*   若用户确实需要访问被标记为 `volatile` 的类的非 `volatile` 成员，可以通过使用 `const_cast` 来进行类型转换。`const_cast` 允许从 `const` 或 `volatile` 类型转换成非 `const` 或非 `volatile` 类型，但要谨慎使用，因为这可能会绕过类型安全机制
*   `volatile` 修饰符会传递给类的成员。如果一个类被声明为 `volatile`，那么它的所有成员（成员变量和成员函数）都会继承 `volatile` 修饰符，除非在成员声明中明确指定不同的修饰符

**多线程下的volatile**

有些变量是用volatile关键字声明的。当两个线程都要用到某一个变量且该变量的值会被改变时，应该用volatile声明，**该关键字的作用是防止优化编译器把变量从内存装入CPU寄存器中**

如果变量被装入寄存器，那么两个线程有可能一个使用内存中的变量，一个使用寄存器中的变量，这会造成程序的错误执行

volatile的意思是让编译器每次操作该变量时一定要从内存中真正取出，而不是使用已经存在寄存器中的值

- **mutable**

在C++中，`mutable` 也是为了突破 `const` 的限制而设置的。被 `mutable` 修饰的变量，将永远处于可变的状态。若类的成员函数不会改变对象的状态，那么这个成员函数一般会声明成 `const` 的。但是若需要**在 `const` 函数里面修改一些跟类状态无关的数据成员，那么这个函数就应该被 `mutable` 来修饰，并且放在函数后后面关键字位置**

```c++
// 样例一
class person
{
    int m_A;
    mutable int m_B;    // 特殊变量 在常函数里值也可以被修改
public:
    void add() const    // 在函数里不可修改this指针指向的值 常量指针
    {
        m_A = 10;       // 错误  不可修改值，this已经被修饰为常量指针
        m_B = 20;       // 正确
    }
};

//  样例二
class person
{
public:
    int m_A;
    mutable int m_B;    // 特殊变量 在常函数里值也可以被修改
};

int main()
{
    const person p = person(); // 修饰常对象 不可修改类成员的值
    p.m_A = 10;     // 错误，被修饰了指针常量
    p.m_B = 200;    // 正确，特殊变量，修饰了mutable
}
```

- **explicit**

`explicit` 关键字用来修饰类的**构造函数**，被修饰的构造函数的类，不能发生相应的隐式类型转换，只能以**显式的方式进行类型转换**，注意以下几点：

*   `explicit` 关键字只能用于类内部的构造函数声明上
*   被 `explicit` 修饰的构造函数的类，不能发生相应的隐式类型转换

---

## 40、什么情况下会调用拷贝构造函数

*   用类的一个实例化对象去**初始化**另一个对象的时候
*   函数的**参数是类的对象**时（非引用传递）
*   函数的**返回值是函数体内局部对象的类的对象**时可能会在返回值的地方调用拷贝构造函数 (返回方式是值传递)。但是编译器可能会进行 Named return Value Optimization (NRVO), 从而不会发生拷贝构造函数

```c++
class A {
public:
    A() {};
    A(const A& a) {
        cout << "copy constructor is called" << endl;
    };
    ~A() {};
};

void useClassA(A a) {}

A getClassA() {
    A a;
    return a;
}

int main() {
    A a1,a3,a4;
    A a2 = a1;      // 调用拷贝构造函数,对应情况1
    useClassA(a1);  // 调用拷贝构造函数，对应情况2
    a3 = getClassA();   // 发生NRV优化，不调用拷贝构造
    return 0;
}
```

**NRVO 是 C++ 中的一种优化技术，用于优化函数返回值的构造过程，由编译器自动执行**

NRVO 的原理是，当一个函数返回一个局部对象时，编译器会尝试将该对象直接构造在调用者提供的内存空间中，而不是在函数的栈帧中创建临时对象，然后再将临时对象拷贝到调用者提供的内存空间。这样做可以避免不必要的拷贝操作，提高了程序的性能

NRVO 可以在满足以下条件时进行优化：

- 函数的返回值是一个非引用类型的对象
- 函数中只有一个返回语句，并且返回的是一个局部对象
- 返回的局部对象和函数的返回类型相同

```c++
#include <iostream>

class MyClass {
public:
    MyClass() { std::cout << "Default constructor called." << std::endl; }
    MyClass(const MyClass& other) { std::cout << "Copy constructor called." << std::endl; }
    MyClass(MyClass&& other) { std::cout << "Move constructor called." << std::endl; }
};

MyClass func() {
    MyClass obj;
    return obj;
}

int main() {
    MyClass result = func(); // 只会输出 Default constructor called.
    return 0;
}
```

---

## 41、C++中有几种类型的new

在C++中，new有三种典型的使用方法：plain new，nothrow new和placement new

（1）**plain new**

plain new 是最常见的 new 使用方法，也是默认的 new 行为，它用于动态地在堆上分配内存，并通过调用对象的构造函数初始化对象，如果内存分配失败，它会抛出 `std::bad_alloc` 异常
在C++中定义如下：

```c++
void* operator new(std::size_t) throw(std::bad_alloc);
void operator delete(void *) throw(); 
// throw() 是异常说明, 示这些函数不会抛出任何异常。在 C++11 之后，异常说明已经被废弃，取而代之的是 noexcept 关键字
```

因此通过判断返回值是否为 nullptr 是徒劳的，举个例子：

```c++
int main() {
    try {
        // 尝试动态分配一个非常大的数组，超过系统可用的内存
        size_t size = 1000000000000000;
        int* pArray = new int[size];
        std::cout << "Dynamic memory allocation succeeded !!" << std::endl;
        delete[] pArray;
    } catch (std::bad_alloc& e) {
        std::cout << "Dynamic memory allocation failed... : " << e.what() << std::endl;
    }
}
//执行结果：Dynamic memory allocation failed... : std::bad_alloc
```

（2）**nothrow new**

`nothrow new` 是一种变体的 new，它用于在堆上分配内存，但是不抛出异常。如果内存分配成功，则返回指向新分配内存的指针；如果分配失败，则返回一个空指针，需要使用 `std::nothrow` 标志来指定使用 `nothrow new`

```c++
void * operator new(std::size_t,const std::nothrow_t&) throw();
void operator delete(void*) throw();

int main() {
    int* pArray = nullptr;
    size_t size = 1000000000000000; 
    // 使用 nothrow new 尝试动态分配内存
    pArray = new(std::nothrow) int[size];
    if (pArray) {
        std::cout << "Dynamic memory allocation succeeded." << std::endl;
        delete[] pArray;
    } else {
        std::cout << "Dynamic memory allocation failed..." << std::endl;
    }
    return 0;
}
``` 

（3）**placement new**

`placement new` 是一种特殊的 new，用于在预先分配的内存块中构造对象，而不是在堆上分配新的内存 (即不会进行内存分配，不用担心内存分配失败)。这种方法可以用于在指定的内存地址上直接创建对象，适用于特定的内存管理需求，例如对象池或自定义内存分配 (即直接调用对象的构造函数在给定的内存地址上构造对象)

使用 `placement new` 需要注意两点：

*   `palcement new` 的主要用途就是反复使用一块较大的动态分配的内存来构造不同类型的对象或者他们的数组
    
*   在使用 `placement new` 构造对象时，需要确保预分配的内存空间足够容纳对象, 并且对象的构造和析构函数需要手动调用，使用 placement new 构造的对象不会自动释放内存。千万不要使用 `delete`，这是因为 `placement new` 构造起来的对象或数组大小并不一定等于原来分配的内存大小，使用delete会造成内存泄漏或者之后释放内存时出现运行时错误。

```c++
void* operator new(size_t,void*);
void operator delete(void*,void*);

class A {
	int i;
public:
	A() {
		i = 10;
		cout << "A construct i = " << i << endl;
	}
	~A() { cout << "A destruct" << endl;}
};

int main() {
	char *p = new(nothrow) char[sizeof(A) + 1];
	if (!p) {
		cout << "alloc failed" << endl;
	}
	A *q = new(p) A;  // placement new:不必担心失败，只要 p 所指对象的的空间足够 A 创建即可
	//delete q; // 错误!不能在此处调用delete q;
	q->~A();    // 显示调用析构函数, 或 q->A::~A();
	delete[] p;
	return 0;
}

// A construct i = 10
// A destruct
```
---

## 42、C++的异常处理的方法

在程序执行过程中，由于程序员的疏忽或是系统资源紧张等因素都有可能导致异常，任何程序都无法保证绝对的稳定，常见的异常有：

*   数组下标越界
*   除法计算时除数为0
*   动态分配空间时空间不足
*   ...

如果不及时对这些异常进行处理，程序多数情况下都会崩溃。

**（1）try、throw和catch关键字**

C++中的异常处理机制主要使用 `try, throw, catch` 三个关键字，其在程序中的用法如下：

```c++
#include <iostream>
#include <stdexcept>

int divide(int a, int b) {
    if (b == 0) {
        throw std::runtime_error("Division by zero is not allowed!");
    }
    return a / b;
}

int main() {
    int x = 10;
    int y = 0;

    try {
        int result = divide(x, y);
        std::cout << "Result: " << result << std::endl;
    } catch (std::exception& e) {
        std::cout << "Exception caught: " << e.what() << std::endl;
    }

    return 0;
}
```

- `try` 块：在可能引发异常的代码块前面使用 `try` 关键字。在 `try` 块中放置可能引发异常的代码。

- `throw` 表达式：当遇到异常情况时，可以使用 `throw` 关键字抛出一个异常。`throw` 后面通常是一个异常对象，可以是标准异常类的对象，也可以是自定义的异常类的对象。(甚至可以是数字)

- `catch` 块：在 `try` 块后面使用 `catch` 关键字来精确捕获 (不会出现类型转换, 如`throw 1` 会被 `catch(int a)` 捕获) 并处理异常。`catch` 后面跟着一个括号，括号内是异常的类型，表示捕获特定类型的异常。当异常被抛出时，程序会在 `catch` 块中查找匹配的异常类型，并执行相应的处理代码。如果匹配不到会直接报错，可以使用 `catch(...)` 的方式捕获任何异常（不推荐, 因为会隐藏真正的异常类型，导致难以调试和定位问题）。当然，如果 `catch` 了异常，当前函数如果不进行处理，或者已经处理了想通知上一层的调用者，可以在 `catch` 里面再 `throw` 异常

**（2）函数的异常声明列表**

有时候，程序员在定义函数的时候知道函数可能发生的异常，可以在函数声明和定义时，指出所能抛出异常的列表，写法如下：

```c++
int fun() throw(int, double, A, B, C) {...};
```
    
这种写法表明函数可能会抛出 int, double 或者 A, B、C 三种类型的异常，如果 throw 中为空，表明不会抛出任何异常, 如果没有throw则可能抛出任何异常
不过表明不抛出异常现在用 `noexcept` 关键字

**（3）C++标准异常类 exception**

C++ 标准库中有一些类代表异常，这些类都是从 exception 类派生而来的，如下图所示

![](http://oss.interviewguide.cn/img/202205212342667.png)

*   `bad_typeid`: `typeid` 运算符可以获取一个对象或者表达式的类型信息。当 `typeid` 运算符的操作数是一个多态类的指针，而且该指针的值为 `nullptr`, 则可能会抛出 `std::bad_typeid` 异常

```c++
#include <iostream>
#include <typeinfo>

class Base {
public:
    virtual void foo() {}
};

class Derived : public Base {
public:
    void foo() override {}
};

int main() {
    Base* ptr = nullptr;
    try {
        const std::type_info& info = typeid(*ptr);
        std::cout << "Type information: " << info.name() << std::endl;
    } catch (const std::bad_typeid& e) {
        std::cout << "Caught std::bad_typeid exception: " << e.what() << std::endl;
    }
    return 0;
}

// Caught std::bad_typeid exception: std::bad_typeid
```

*   `bad_cast`: 在用 `dynamic_cast` 进行从多态基类对象（或引用）到派生类的引用的强制类型转换时，如果转换是不安全的，则会拋出此异常

```c++
#include <iostream>
#include <typeinfo>
#include <exception>

class Base {
public:
    virtual void foo() {}
};

class Derived : public Base {
public:
    void foo() override {}
};

int main() {
    Base baseObj;
    Base* basePtr = &baseObj;

    try {
        Derived& derivedRef = dynamic_cast<Derived&>(*basePtr);
        derivedRef.foo();
    } catch (const std::bad_cast& e) {
        std::cout << "Caught std::bad_cast exception: " << e.what() << std::endl;
    }

    return 0;
}

// Caught std::bad_cast exception: std::bad_cast
```

*   `bad_alloc`: 在用 `new` 运算符进行动态内存分配时，如果没有足够的内存，则会引发此异常
*   `out_of_range`: 用 `vector` 或 `string` 的 `at` 成员函数根据下标访问元素时，如果下标越界，则会拋出此异常

---

## 43、static的用法和作用？

1.先来介绍它的第一条也是最重要的一条：**隐藏**（static函数，static变量均可）

当同时编译多个文件时，所有未加static前缀的全局变量和函数都具有全局可见性。

2.static的第二个作用是**保持变量内容的持久**。（static变量中的记忆功能和全局生存期）存储在静态数据区的变量会在程序刚开始运行时就完成初始化，也是唯一的一次初始化。**共有两种变量存储在静态存储区：全局变量和static变量，只不过和全局变量比起来，static可以控制变量的可见范围，说到底static还是用来隐藏的**

3.static的第三个作用是**默认初始化为0**（static变量）

其实全局变量也具备这一属性，因为全局变量也存储在静态数据区。在静态数据区，内存中所有的字节默认值都是0x00，某些时候这一特点可以减少程序员的工作量

4.static的第四个作用：C++中的**类成员声明 static**

1.  函数体内 static 变量的作用范围为该函数体，不同于auto变量，该变量的内存只被分配一次，因此其值在下次调用时仍维持上次的值；
    
2.  在模块内的 static 全局变量可以被模块内所有函数访问，但不能被模块外其它函数访问；
    
3.  在模块内的 static 函数只可被这一模块内的其它函数调用，这个函数的使用范围被限制在声明它的模块内；
    
4.  在类中的 static 成员变量属于整个类所拥有，对类的所有对象只有一份拷贝；
    
5.  在类中的 static 成员函数属于整个类所拥有，这个函数不接收this指针，因而只能访问类的 static 成员变量。

6.  static类对象必须要在类外进行初始化，**static 修饰的变量先于对象存在，所以 static 修饰的变量要在类外初始化**；
    
7.  由于static修饰的类成员属于类，不属于对象，因此static类成员函数是没有this指针的，this指针是指向本对象的指针。正因为没有this指针，所以**static类成员函数不能访问非static的类成员，只能访问 static修饰的类成员**；
    
8.  **static成员函数不能被virtual修饰，static成员不属于任何对象或实例，所以加上virtual没有任何实际意义**；静态成员函数没有this指针，虚函数的实现是为每一个对象分配一个 vptr 指针，而 vptr 是通过 this 指针调用的，所以不能为 virtual；**虚函数的调用关系，this->vptr->vtable->virtual function**
    
---

## 44、指针和const的用法

1.  当 const 修饰指针时，由于const的位置不同，它的修饰对象会有所不同。
    
2.  `int *const p2` 中 `const` 修饰 p2 的值,所以理解为 p2 的值不可以改变，即 p2 只能指向固定的一个变量地址，但可以通过 `*p2` 读写这个变量的值。顶层指针表示指针本身是一个常量
    
3.  `int const *p1` 或者 `const int *p1` 两种情况中 const 修饰 `*p1`，所以理解为 `*p1` 的值不可以改变，即不可以给 `*p1` 赋值改变指向变量的值，但可以通过给 p1 赋值不同的地址改变这个指针指向。底层指针表示指针所指向的变量是一个常量

---

## 45、形参与实参的区别？

1.  **形参变量只有在被调用时才分配内存单元，在调用结束时， 即刻释放所分配的内存单元**。因此，形参只有在函数内部有效。 函数调用结束返回主调函数后则不能再使用该形参变量。
    
2.  **实参可以是常量、变量、表达式、函数等， 无论实参是何种类型的量，在进行函数调用时，它们都必须具有确定的值**， 以便把这些值传送给形参。 因此应预先用赋值，输入等办法使实参获得确定值，会产生一个临时变量。
    
3.  实参和形参在数量上，类型上，顺序上应严格一致， 否则会发生“类型不匹配”的错误
    
4.  函数调用中发生的数据传送是单向的。 即只能把实参的值传送给形参，而不能把形参的值反向地传送给实参。 因此在函数调用过程中，形参的值发生改变，而实参中的值不会变化。
    
5.  当形参和实参不是指针类型时，在该函数运行时，形参和实参是不同的变量，他们在内存中位于不同的位置，形参将实参的内容复制一份，在该函数运行结束的时候形参被释放，而实参内容不会改变

---

## 46、值传递、指针传递、引用传递的区别和效率

1.  值传递：有一个实参向函数所属的栈拷贝数据的过程，如果值传递的对象是类对象 或是大的结构体对象，将耗费一定的时间和空间。（传值）
    
2.  指针传递：同样有一个实参向函数所属的栈拷贝数据的过程，但拷贝的数据是一个固定大小的地址 (4或8字节)。（传值，传递的是地址值）
    
3.  引用传递：同样有上述的数据拷贝过程，但其是针对地址的，相当于为该数据所在的地址起了一个别名。（传地址）
    
4.  效率上讲，指针传递和引用传递比值传递效率高。一般主张使用引用传递 (效率最高并且可以直接修改实参)，代码逻辑上更加紧凑、清晰。

---

## 47、静态变量什么时候初始化

1.  **初始化只有一次，但是可以多次赋值，在主程序之前，编译器已经为其分配好了内存**
    
2.  静态局部变量和全局变量一样，数据都存放在全局区域，所以在主程序之前，编译器已经为其分配好了内存，但在C和C++中静态局部变量的初始化节点又有点不太一样。在C中，初始化发生在代码执行之前，编译阶段分配好内存之后，就会进行初始化，所以我们看到在**C语言中无法使用变量对静态局部变量进行初始化**，在程序运行结束，变量所处的全局内存会被全部回收。
    
3.  而在C++中，初始化时在执行相关代码时才会进行初始化，主要是由于C++引入对象后，要进行初始化必须执行相应构造函数和析构函数，在构造函数或析构函数中经常会需要进行某些程序中需要进行的特定操作，并非简单地分配内存。所以C++标准定为全局或静态对象是有首次用到时才会进行构造，并通过 `atexit()` 来管理。在程序结束，按照构造顺序反方向进行逐个析构。所以在**C++中是可以使用变量对静态局部变量进行初始化的**。

---

## 48、const关键字的作用有哪些?

1. 阻止变量被改变：使用const关键字可以将变量声明为常量，阻止其在后续的程序中被修改，因此必须在定义的时候进行初始化

2. const指针和指针指向的数据：const可以修饰指针本身，也可以修饰指针指向的数据，或者同时修饰指针和指向的数据。

3. 函数中的const参数：const可以修饰函数的形参，表明这是一个输入参数，在函数内部不能修改其值。

4. const成员函数：const修饰的成员函数称为常函数，它不能修改类的成员变量，**常对象只能调用常成员函数**

5. const返回值：有时候需要将成员函数的返回值声明为const类型，以确保返回值不为“左值”，提供更多的安全性。

6. const成员函数访问权限：const成员函数可以访问非const对象的所有数据成员，也可以访问const对象内的所有数据成员 (常对象和非常对象都可以调用常成员函数)

7. 非const成员函数访问权限：非const成员函数可以访问非const对象的所有数据成员 (包括 const 变量)，但不可以访问const对象的任意数据成员 (常对象不能调用常成员函数, 因为没有明确声明为const的成员函数被看作是将要修改对象中数据成员的函数，所以编译器不允许它为一个const对象所调用)

8. const_cast：const类型变量可以通过 `const_cast` 转换为非const类型，但要慎用，因为它可能导致未定义行为。

9. 函数传参：对于函数值传递的情况，因为本身就无法改变实参，加不加const对实参不会产生任何影响。但是在传引用或指针的函数中，const 可以保护实参所指向的变量。因为在编译阶段编译器对调用函数的选择是根据实参进行的，所以，**只有引用传递和指针传递可以用是否加const来重载。一个拥有顶层const的形参无法和另一个没有顶层const的形参区分开来**

---    

## 49、什么是类的继承？

1.  类与类之间的关系

has-A 包含关系，用以描述一个类由多个部件类构成，实现has-A关系用类的成员属性表示，即一个类的成员属性是另一个已经定义好的类；

use-A，一个类使用另一个类，通过类之间的成员函数相互联系，定义友元或者通过传递参数的方式来实现；

is-A，继承关系，关系具有传递性；

2.  继承的相关概念

所谓的继承就是一个类继承了另一个类的属性和方法，这个新的类包含了上一个类的属性和方法，被称为子类或者派生类，被继承的类称为父类或者基类；

3.  继承的特点

**子类拥有父类的所有属性和方法，子类可以拥有父类没有的属性和方法，子类对象可以当做父类对象使用；**

4.  继承中的访问控制

public、protected、private

5.  继承中的构造和析构函数
    
6.  继承中的兼容性原则
    
---

## 50、从汇编层去解释一下引用

```c++
9:      int x = 1;

00401048  mov     dword ptr [ebp-4],1

10:     int &b = x;

0040104F   lea     eax,[ebp-4]

00401052  mov     dword ptr [ebp-8],eax
```

x的地址为 ebp-4，b的地址为 ebp-8，因为栈内的变量内存是从高往低进行分配的，所以b的地址比x的低。

`lea eax,[ebp-4]` 这条语句将x的地址 ebp-4 放入 eax 寄存器

`mov dword ptr [ebp-8],eax` 这条语句将 eax 的值放入b的地址

ebp-8 中上面两条汇编的作用即：将x的地址存入变量b中，这不和将某个变量的地址存入指针变量是一样的吗？**所以从汇编层次来看，引用是通过指针来实现的**

---

## 51、深拷贝与浅拷可以描述一下吗？

深拷贝（Deep Copy）和浅拷贝（Shallow Copy）是关于对象或数据的复制方式的概念

浅拷贝是指简单地将一个对象的数据成员的值复制到另一个对象中，这样两个对象会**共享相同的内存地址**，如果其中一个对象修改了共享的数据成员，另一个对象也会受到影响。通常，浅拷贝是通过默认的拷贝构造函数或赋值运算符实现的。

深拷贝是指在复制对象时，将对象的数据成员值复制到一个新的内存地址中，从而使得两个对象拥有独立的内存空间。这样，如果其中一个对象修改了数据成员，另一个对象不会受到影响。深拷贝通常需要自定义拷贝构造函数和赋值运算符，以确保每个对象都有自己的独立副本。

浅拷贝中假设 A 的成员申请了一块内存, B = A 的话会让 B 中的成员也指向该内存。那么若 A 把内存释放了 (如：析构), 这时 B 内的指针就是悬挂指针了，出现运行错误

**总结：**

- 浅拷贝只复制指针，导致两个对象共享相同的数据，容易引发悬挂指针

- 深拷贝复制数据本身，每个对象都有独立的数据，更安全可靠。但需要注意释放内存，避免内存泄漏

---

## 52、new和malloc的区别

1、 `new/delete` 是 C++关键字，需要**编译器**支持。`malloc/free` 是库函数，需要**头文件**支持；

2、 使用new操作符申请内存分配时无须指定内存块的大小，编译器会根据类型信息自行计算。而malloc则需要显式地指出所需内存的尺寸。

3、 new操作符内存分配成功时，返回的是对象类型的指针，类型严格与对象匹配，无须进行类型转换，故 **new 是符合类型安全性的操作符**。而 `malloc` 内存分配成功则是返回 `void *` ，需要通过强制类型转换将指针转换成我们需要的类型。

4、 `new` 内存分配失败时，会抛出 `bac_alloc` 异常。`malloc` 分配内存失败时返回 `NULL`

5、 `new` 会先调用 `operator new` 函数，申请足够的内存（通常底层使用 `malloc` 实现）。然后调用类型的构造函数，初始化成员变量，最后返回自定义类型指针。`delete` 先调用析构函数，然后调用 `operator delete` 函数释放内存（通常底层使用 `free` 实现）。`malloc/free` 是库函数，只能动态的申请和释放内存，无法强制要求其做自定义类型对象构造和析构工作。

---

## 53、delete p、delete[] p、allocator都有什么作用？

`delete p, delete[] p` 都是用于释放动态分配的内存的；不过 `delete p` 释放 `new` 运算符分配的单个对象的内存，`delete[] p` 释放使用 `new[]` 运算符分配的数组的内存 (数组中的元素按**逆序**的顺序进行销毁)

`allocator`: C++ 标准库中的 `allocator` 类模板用于动态分配内存，它允许在堆上分配空间来存储对象，而不需要显式地调用 new 和 delete。它为容器类提供了内存管理，用于分配和释放存储在容器中的对象

```c++
#include <vector>
#include <memory>

std::vector<int, std::allocator<int>> vec; // 使用allocator为vector分配内存
vec.push_back(10); // 将10添加到vector中
// 不需要显式的delete，vector在销毁时会自动释放内存
```

注意
1、 动态数组管理 `new` 一个数组时，`[]` 中必须是一个整数，但是不一定是常量整数 (这意味着数组大小可以是在运行时计算得出的，而不仅仅是在编译时就确定的常量); 普通数组必须是一个常量整数；`new` 动态数组返回的并不是数组类型，而是一个元素类型的指针；

2、 `new` 在内存分配上面有一些局限性，`new` 的机制是将内存分配和对象构造组合在一起，同样的，`delete` 也是将对象析构和内存释放组合在一起的

3、 `allocator` 将这两部分分开进行，它只负责内存的分配和释放，不涉及对象的构造和析构。通过 allocator 分配的内存是未初始化的原始内存块，只有在需要的时候才调用构造函数来初始化对象

---

## 54、new 和 delete 的实现原理， delete 是如何知道释放内存的大小的？

- **new**

`new` 内置类型直接调用 `operator new` 分配内存； 而对于自定义类型，先调用 `operator new` 分配内存，然后在分配的内存上调用构造函数；

对于内置类型，`new[]` 计算好大小后调用 `operator new`；

对于自定义类型，`new[]` 先调用 `operator new[]` 分配内存，然后在 `ptr` 的前四个字节写入数组大小 n，然后调用 n 次构造函数，**针对自定义类型，`new[]` 会额外存储数组大小；**

① `new` 表达式调用一个名为 `operator new (operator new[])` 函数，分配一块足够大的、原始的、未命名的内存空间；

② 编译器运行相应的构造函数以构造这些对象，并为其传入初始值；

③ 对象被分配了空间并构造完成，返回一个指向该对象的指针 ptr

- **delete**
  
`delete` 内置类型默认只是调用 `free` 函数；自定义类型先调用析构函数再调用 `operator delete`；

针对内置类型，`delete` 和 `delete[]` 等同。
对于自定义类型而言，假设指针 ptr 指向 `new[]` 分配的内存。因为要4字节存储数组大小，实际分配的内存地址为 `ptr - 4`，系统记录的也是这个地址。`delete[]` 实际释放的就是 `ptr - 4` 指向的内存。而 `delete` 会直接释放 `ptr` 指向的内存，这个内存根本没有被系统记录，所以会崩溃

为了确定需要调用多少次析构函数，C++ 的做法是在调用 `new[]` 分配数组空间时多分配 4 个字节的大小来专门保存数组的大小，在 `delete[]` 时就可以取出这个保存的数，就知道了需要调用析构函数多少次了

---

## 55、malloc申请的存储空间能用delete释放吗?

不能，`malloc /free` 主要为了兼容C，`new / delete` 完全可以取代 `malloc /free` 的

`malloc /free` 的操作对象都是必须明确大小的，而且不能用在动态类上

`new / delete` 会自动进行类型检查和大小，`malloc / free` 不能执行构造函数与析构函数，所以动态对象它是不行的

当然从理论上说使用 `malloc` 申请的内存是可以通过 `delete` 释放的。不过一般不这样写的。而且也不能保证每个C++的运行时都能正常

---

## 56、malloc与free的实现原理？

1、 在标准C库中，提供了 `malloc/free` 函数分配释放内存，这两个函数底层是由 `brk, mmap, munmap` 这些系统调用实现的;

2、 `brk` 是将数据段 `(.data)` 的最高地址指针 `_edata` 往高地址推; `mmap` 是在进程的虚拟地址空间中（堆和栈中间，称为文件映射区域的地方）找一块空闲的虚拟内存。这两种方式**分配的都是虚拟内存，没有分配物理内存**。在第一次访问已分配的虚拟地址空间的时候，发生缺页中断，操作系统负责分配物理内存，然后建立虚拟内存和物理内存之间的映射关系；

3、 malloc 小于128k的内存，使用 brk 分配内存，将 _edata 往高地址推；malloc 大于128k的内存，使用mmap 分配内存，在堆和栈之间找一块空闲内存分配；**brk 分配的内存需要等到高地址内存释放以后才能释放(因为数据段是连续的一段内存)，而 mmap 分配的内存可以单独释放**。`.data` 段在最高地址空间的空闲内存超过128K（可由 `M_TRIM_THRESHOLD` 选项调节）时，执行内存紧缩操作 (`trim`)

4、 malloc 是从堆里面申请内存，也就是说函数返回的指针是指向堆里面的一块内存。操作系统中有一个记录空闲内存地址的链表。当操作系统收到程序的申请时，就会遍历该链表，然后就寻找第一个空间大于所申请空间的堆结点，然后就将该结点从空闲结点链表中删除，并将该结点的空间分配给程序

---

## 57、malloc、realloc、calloc的区别

1.  malloc函数

```c++
void* malloc(unsigned int num_size);
int *p = malloc(20*sizeof(int)); // 申请20个int类型的空间
```

2.  calloc函数

```c++
void* calloc(size_t n,size_t size);
int *p = calloc(20, sizeof(int));
```
省去了人为空间计算；malloc 申请的空间的值是随机初始化的，calloc 申请的空间的值是初始化为0的；

3.  realloc函数

```c++
void realloc(void *p, size_t new_size);
```
给动态分配的空间分配额外的空间，用于扩充容量

---

## 58、类成员初始化方式？构造函数的执行顺序 ？为什么用成员初始化列表会快一些？

- 列表初始化，在冒号后使用初始化列表进行初始化。
- 赋值初始化，通过在函数体内进行赋值初始化；

**正常构造函数的执行顺序：**

- 开辟内存空间
- 按照成员变量在类中的声明顺序构造成员变量
  - 如果成员变量在初始化列表中, 就会执行该变量类型的拷贝构造函数; 
  - 如果成员变量没有在初始化列表中, 就会执行该变量类型的缺省构造函数.
- 最后执行构造函数体内的代码

**为什么使用成员初始化列表会快一些**

**使用成员初始化列表可以在对象创建时直接初始化成员变量，而不需要先调用默认构造函数创建对象后再进行赋值操作**。这样可以减少一次对象的构造和赋值过程，从而提高了代码的执行效率。**对于在函数体中初始化, 是在所有的数据成员被分配内存空间后才进行的**

另外，**对于一些成员变量是 const 或引用类型的情况，它们必须在构造函数初始化列表中进行初始化，而不能在构造函数体内进行赋值初始化**。因为const成员变量和引用类型必须在创建对象时就进行初始化，并且不能重新赋值。因此，使用成员初始化列表是必须的，而且也更加高效。


**一个派生类构造函数的执行顺序**

① 虚拟基类的构造函数（多个虚拟基类则按照继承的顺序执行构造函数）

② 基类的构造函数（多个普通基类也按照继承的顺序执行构造函数）

③ 类类型的成员对象的构造函数（按照成员对象在类中的定义顺序）

④ 派生类自己的构造函数

赋值初始化是在构造函数当中做赋值的操作，而列表初始化是做纯粹的初始化操作。我们都知道，C++的赋值操作是会产生临时对象的。临时对象的出现会降低程序的效率

---

## 59、有哪些情况必须用到成员列表初始化？作用是什么？

1.  必须使用成员初始化的四种情况

    - 当初始化一个引用成员时；

    - 当初始化一个常量成员时；

    - 当调用一个基类的构造函数，而它拥有一组参数时；

    - 当调用一个成员类的构造函数，而它拥有一组参数时；


2. 成员初始化列表的主要作用是：

   - 提供一种在构造函数执行之前初始化类成员的方式，确保类成员在构造函数体内可用。

   - 可以对类成员进行复杂的初始化操作，包括调用其他构造函数、传递参数等，以满足不同初始化需求。

   - 帮助避免使用默认初始化，从而提高代码效率和可读性

---

## 60、C++中新增了string，它与C语言中的 `char *` 有什么区别吗？它是如何实现的？

- 数据结构：C语言中的 `char*` 是一个字符指针，指向以 `null` 结尾的字符数组，而C++中的 `string` 是一个类，封装了字符串的操作和管理，它可以动态调整字符串的长度，并且不需要手动管理内存。

- 动态内存管理：C风格字符串需要手动分配内存和释放内存，容易出现内存泄漏或越界访问的问题。而C++中的 string 类自动管理字符串的内存，它会根据需要自动分配和释放内存，大大简化了内存管理。

- 字符串操作：C风格字符串的操作需要调用C标准库中的字符串函数，例如`strcpy、strlen、strcat`等，操作不够直观和安全。而C++中的 string 类提供了丰富的字符串操作方法，例如 `append、substr、find` 等，使字符串操作更加方便和安全。

- 安全性：C风格字符串没有越界检查，容易引发程序崩溃和安全漏洞。而C++中的 string 类具有越界检查机制，可以更好地保障程序的安全性。

一般情况下，`string` 类内部使用动态分配的字符数组来存储字符串，同时还包含了字符串的长度容量等信息。`string` 类还提供了许多成员函数来操作和管理字符串，这些函数的实现会涉及到字符数组的动态分配和释放、字符串的复制和拼接等操作。总体上，string 类的实现是为了提供更高效、更方便、更安全的字符串操作功能。

string 可以进行动态扩展，在每次扩展的时候另外申请一块原空间大小两倍的空间（2 * n），然后将原字符串拷贝过去，并加上新增的内容。

---

## 61、什么是内存泄露，如何检测与避免

**内存泄露**

一般我们常说的内存泄漏是指**堆内存的泄漏**。堆内存是指程序从堆中分配的，大小任意的(内存块的大小可以在程序运行期决定)内存块，使用完后必须显式释放的内存。应用程序一般使用malloc,、realloc、 new等函数从堆中分配到块内存，使用完后，程序必须负责相应的调用free或delete释放该内存块，否则，这块内存就不能被再次使用，我们就说这块内存泄漏了

**避免内存泄露的几种方式**

*   计数法：使用new或者malloc时，让该数+1，delete或free时，该数-1，程序执行完打印这个计数，如果不为0则表示存在内存泄露
*   一定要将基类的析构函数声明为**虚函数**
*   对象数组的释放一定要用**delete []**
*   有new就有delete，有malloc就有free，保证它们一定成对出现

**检测工具**

*   Linux下可以使用**Valgrind工具**
*   Windows下可以使用**CRT库**

---

## 62、对象复用的了解，零拷贝的了解

**对象复用**

对象复用其本质是一种设计模式：享元模式, 是一种结构型设计模式，它旨在通过共享对象来减少内存使用和提高性能

在享元模式中，将对象分为两种类型：内部状态（Intrinsic State）和外部状态（Extrinsic State）。内部状态是对象的共享部分，它存储在对象内部，并且可以被多个对象共享。外部状态是对象的非共享部分，它取决于对象的环境，不同的环境会导致对象的行为和外部状态发生变化。

对象复用通过将内部状态共享，减少重复创建对象的开销。具体实现时，可以使用对象池来存储和管理对象。对象池维护一个空闲对象的集合，当需要使用对象时，首先从对象池中获取一个空闲对象，如果对象池为空，则创建新的对象并放入池中。在对象不再需要时，将对象归还给对象池，以供下次复用。

对象复用在一些场景下特别有用，例如在频繁创建和销毁对象的情况下，可以显著提高性能和降低内存消耗。常见的应用场景包括连接池、线程池、数据库连接池等。

**总结起来，对象复用是通过享元模式和对象池的技术手段来实现的，它可以有效地减少对象创建和销毁的开销，提高系统性能和资源利用率**

**零拷贝**

零拷贝就是一种避免 CPU 将数据从一块存储拷贝到另外一块存储的技术

零拷贝技术可以减少数据拷贝和共享总线操作的次数

在C++中，vector的一个成员函数 `emplace_back()` 很好地体现了零拷贝技术，它跟 `push_back()` 函数一样可以将一个元素插入容器尾部，区别在于：**使用 `push_back()` 函数需要调用拷贝构造函数和移动构造函数，而使用 `emplace_back()` 插入的元素原地构造，不需要触发拷贝构造和移动构造**，效率更高。举个例子：

```c++
struct Person {
    string name;
    int age;
    Person() = default;
    Person(string p_name, int p_age): name(p_name), age(p_age) {
         cout << "construct" <<endl;
    }

    Person(const Person& other): name(other.name), age(other.age) {
         cout << "copy construct" <<endl;
    }

    Person(Person&& other) : name(std::move(other.name)), age(other.age) {
         cout << "move construct"<<endl;
    }
};

int main() {
    vector<Person> e;
    cout << e.capacity() << endl; // 0
    e.emplace_back("Mike",36); // construct
    cout << e.capacity() << endl; // 1
    e.emplace_back("casey",28); // construct, copy construct
    cout << e.capacity() << endl; // 2
    e.push_back(Person("alice",18)); // construct, move construct, copy construct, copy construct
    cout << e.capacity() << endl; // 4
    e.push_back(Person("James",21)); // construct, move construct
    cout << e.capacity() << endl; // 4
    return 0;
}
```

`push_back()` 会先调用构造函数创建出一个临时对象, 再调用移动构造插入数组。注意若此时数组空间不够了会发生扩容操作, 会先构造新的再移动旧的元素 (若移动构造函数加了 `noexcept` 的话, 就不会调用拷贝构造函数来拷贝旧元素了而是直接调用移动构造函数)

## 63、介绍面向对象的三大特性，并且举例说明

三大特性：封装、继承和多态

**（1）封装**

数据和代码捆绑在一起，避免外界干扰和不确定性访问

封装，也就是**把客观事物封装成抽象的类**，并且类可以把自己的数据和方法只让可信的类或者对象操作，对不可信的进行信息隐藏，例如：将公共的数据或方法使用public修饰，而不希望被访问的数据或方法采用private修饰。

**（2）继承**

**让某种类型对象获得另一个类型对象的属性和方法。**

它可以使用现有类的所有功能，并在无需重新编写原来的类的情况下对这些功能进行扩展

常见的继承有三种方式：

1.  实现继承：指使用基类的属性和方法而无需额外编码的能力
2.  接口继承：指仅使用属性和方法的名称、但是子类必须提供实现的能力
3.  可视继承：指子窗体（类）使用基窗体（类）的外观和实现代码的能力（C++里好像不怎么用）

例如，将人定义为一个抽象类，拥有姓名、性别、年龄等公共属性，吃饭、睡觉、走路等公共方法，在定义一个具体的人时，就可以继承这个抽象类，既保留了公共属性和方法，也可以在此基础上扩展跳舞、唱歌等特有方法


**（3）多态**

同一事物表现出不同事物的能力，即向不同对象发送同一消息，不同的对象在接收时会产生不同的行为. **重载实现编译时多态，虚函数实现运行时多态**

多态性是允许你将父对象设置成为和一个或更多的他的子对象相等的技术，赋值之后，父对象就可以根据当前赋值给它的子对象的特性以不同的方式运作。**简单一句话：允许将子类类型的指针赋值给父类类型的指针**

实现多态有二种方式：覆盖（override），重载（overload）。

覆盖：是指子类重新定义父类的虚函数的做法。

重载：是指允许存在多个同名函数，而这些函数的参数表不同（或许参数个数不同，或许参数类型不同，或许两者都不同）。例如：基类是一个抽象对象——人，那教师、运动员也是人，而使用这个抽象对象既可以表示教师、也可以表示运动员。

---

## 64、成员初始化列表的概念，为什么用它会快一些？

**成员初始化列表的概念**

在类的构造函数中，不在函数体内对成员变量赋值，而是在构造函数的花括号前面使用冒号和初始化列表赋值

**效率**

用初始化列表会快一些的原因是，对于类类型，它少了一次调用构造函数的过程，而在函数体中赋值则会多一次调用。而对于内置数据类型则没有差别。举个例子：

```c++
class A {
public:
    A() { cout << "默认构造函数A()" << endl; }

    A(int a) {
        value = a;
        cout << "A(int "<<value<<")" << endl;
    }

    A(const A& a) {
        value = a.value;
        cout << "拷贝构造函数A(A& a):  "<<value << endl;
    }
    int value;
};

class B {
public:
    B() : a(1) {
        b = A(2);
    }
    A a, b;
};

int main() {
    B b;
}

// A(int 1)
// 默认构造函数A()
// A(int 2)
```

由于对象成员变量的初始化动作发生在进入构造函数体之前，对于内置类型没什么影响，但**如果有些成员是类**，那么在进入构造函数之前，会先调用一次默认构造函数，进入构造函数后所做的事其实是一次赋值操作(对象已存在)，所以**如果是在构造函数体内进行赋值的话，等于是一次默认构造加一次赋值，而初始化列表只做一次赋值操作**

---

## 65、C++的四种强制转换 `reinterpret_cast, const_cast, static_cast, dynamic_cast`

- **`reinterpret_cast`**: `reinterpret_cast<type-id> (expression)`

type-id 必须是一个指针、引用、算术类型、函数指针或者成员指针。它可以用于类型之间进行强制转换。`reinterpret_cast` 是一种非常危险的类型转换，应该谨慎使用。因为它会绕过类型系统的检查，可能导致未定义行为和安全性问题

- **const_cast**: `const_cast<type_id> (expression)`

`const_cast` 是一种 C++ 运算符，主要是用来去除复合类型中 `const` 和 `volatile` 属性。变量本身的 const 属性是不能去除的，要想修改变量的值，一般是去除指针（或引用）的const属性，再进行间接修。改除了const 或 volatile 修饰之外， type_id 和 expression 的类型是一样的。用法如下：

*   常量指针被转化成非常量的指针，并且仍然指向原来的对象
    
*   常量引用被转换成非常量的引用，并且仍然指向原来的对象
    
*   `const_cast` 一般用于修改底层 `const`; 可以改 this 来达到在常成员函数中修改成员变量的目的

```c++
class Test {
public:
	Test() : val(0) {}
	void fun() const { // this 指针相当于 const Test* const this
		// val = 10; // 错误
		const_cast<Test*>(this)->val = 10; // OK
	}
	int val;
};

int main() {
    Test test;
    cout << test.val << endl; // 0
    test.fun();
    cout << test.val << endl; // 10
}
```

- **static_cast**: `static_cast<type-id> (expression)`

该运算符把expression转换为type-id类型，但**没有运行时类型检查来保证转换的安全性**。它主要有如下几种用法：

*   用于类层次结构中基类（父类）和派生类（子类）之间指针或引用引用的转换
    
    *   进行上行转换（把派生类的指针或引用转换成基类表示）是安全的
        
    *   进行下行转换（把基类指针或引用转换成派生类表示）时，由于没有动态类型检查，所以是不安全的
        
*   用于基本数据类型之间的转换，如把 int 转换成 char，把 int 转换成 enum。这种转换的安全性也要开发人员来保证。
    
*   把空指针转换成目标类型的空指针
    
*   把任何类型的表达式转换成void类型
    

**注意：static_cast 不能转换掉 expression 的 const、volatile、或者 __unaligned 属性(__unaligned 是 Microsoft Visual C++ 编译器的一个扩展，用于告诉编译器在处理结构体或对象时，不要要求其成员按照对齐方式对齐)**

- **dynamic_cast**: `dynamic_cast<type-id> (expression)`

该运算符把 expression 转换成 type-id 类型的对象。type-id 必须是类的指针、类的引用或者void *, 有类型检查。**`dynamic_cast` 主要用于类层次间的上行转换和下行转换，还可以用于类之间的交叉转换**

如果 type-id 是类指针类型，那么expression也必须是一个指针，如果 type-id 是一个引用，那么 expression 也必须是一个引用

**`dynamic_cast` 运算符可以在执行期决定真正的类型，也就是说 expression 必须是多态类型**。如果下行转换是安全的（也就说，如果基类指针或者引用确实指向一个派生类对象）这个运算符会传回适当转型过的指针。**如果下行转换不安全，这个运算符会传回空指针**（也就是说，基类指针或者引用没有指向一个派生类对象）

在类层次间进行上行转换时，`dynamic_cast` 和 `static_cast` 的效果是一样的; **在进行下行转换时，dynamic_cast具有类型检查的功能，比static_cast更安全**，而使用static_cast下行转换存在不安全的情况也可以转换成功，但是直接使用转换后的对象进行操作容易造成错误

---

## 66、C++函数调用的压栈过程

```c++
int f(int n) {
    cout << n << endl;
    return n;
}

void func(int param1, int param2) {
    int var1 = param1;
    int var2 = param2;
    printf("var1 = %d, var2 = %d\n", f(var1), f(var2)); // 如果将 printf 换为 cout 进行输出，输出结果则刚好相反
    cout << "var1 = " << f(var1) << ", var2 = " << f(var2) << endl;
}

int main(int argc, char* argv[]) {
    func(1, 2);
    return 0;
}
// 2
// 1
// var1 = 1, var2 = 2
// var1 = 1
// 1, var2 = 2
// 2
```

- 文字描述

当函数从入口函数 main 函数开始执行时，编译器会将我们操作系统的运行状态，main 函数的返回地址、main 的参数、mian 函数中的变量、进行依次压栈；

当main函数开始调用 `func()` 函数时，编译器此时会将 mai n函数的运行状态进行压栈，再将 `func()` 函数的返回地址、`func()` 函数的参数从右到左、`func()` 定义变量依次压栈；

当 `func()` 调用 `f()` 的时候，编译器此时会将 `func()` 函数的运行状态进行压栈，再将 `f()` 的返回地址、`f()` 函数的参数从右到左、`f()` 定义的变量依次压栈

从代码的输出结果可以看出，函数 `f(var1), f(var2)` 依次入栈，而后先执行 `f(var2)`, 再执行 `f(var1)`, 最后打印整个字符串，将栈中的变量依次弹出，最后主函数返回

`cout` 的执行是从前往后依次执行的 (`inline std::ostream &std::operator<<<std::char_traits<char>>(std::ostream &__out, const char *__s)`), 所以输出 `"var1 = "` 后 `f(var1)` 入栈, 输出 `"1\n"`, 返回 `func()` 并输出 `f(var1)` 的值, 再输出 `", var2 = "`, `f(var2)` 同上

- **函数的调用过程：**

1）从栈空间分配存储空间

2）从实参的存储空间复制值到形参栈空间

3）进行运算

**形参在函数未调用之前都是没有分配存储空间的，在函数调用结束之后，形参弹出栈空间，清除形参空间**

数组作为参数的函数调用方式是地址传递，形参和实参都指向相同的内存空间，调用完成后，形参指针被销毁，但是所指向的内存空间依然存在，不能也不会被销毁。

当函数有多个返回值的时候，不能用普通的 return 的方式实现，需要通过传回地址的形式进行，即地址/指针传递。

---

## 67、写C++代码时有一类错误是 coredump ，很常见，你遇到过吗？怎么调试这个错误？

coredump是程序由于异常或者bug在运行时异常退出或者终止，在一定的条件下生成的一个叫做core的文件，这个 core 文件会记录程序在运行时的内存，寄存器状态，内存指针和函数堆栈信息等等。对这个文件进行分析可以定位到程序异常的时候对应的堆栈调用信息。

*   使用gdb命令对core文件进行调试

> mkdir coredumpTest
> vim coredumpTest.cpp

```c
#include<stdio.h>
int main() {
    int i;
    scanf("%d", i); // 正确的应该是 &i,这里使用 i 会导致 segment fault
    printf("%d\n", i);
    return 0;
}
```

> g++ coredumpTest.cpp -g -o coredumpTest // 编译，注意 -g
> ./coredumpTest // 运行
> gdb [可执行文件名] [core文件名] // 使用 gdb 调试 coredump
    
---

## 68、说说移动构造函数

1.  我们用对象 a 初始化对象 b 后对象 a 我们就不在使用了，但是对象 a 的空间还在呀（在析构之前），既然拷贝构造函数，实际上就是把 a 对象的内容复制一份到 b 中，那么为什么我们不能直接使用 a 的空间呢？这样就避免了新的空间的分配，大大降低了构造的成本。这就是移动构造函数设计的初衷；
    
2.  拷贝构造函数中，对于指针，我们一定要采用深层复制，而移动构造函数中，对于指针，我们采用浅层复制。浅层复制之所以危险，是因为两个指针共同指向一片内存空间，若第一个指针将其释放，另一个指针的指向就不合法了
    
所以我们只要避免第一个指针释放空间就可以了。避免的方法就是将第一个指针（比如a->value）置为NULL，这样在调用析构函数的时候，由于有判断是否为NULL的语句，所以析构a的时候并不会回收a->value指向的空间；

3.  移动构造函数的参数和拷贝构造函数不同，拷贝构造函数的参数是一个左值引用，但是移动构造函数的初值是一个右值引用。意味着，移动构造函数的参数是一个右值或者将亡值的引用。也就是说，只用用一个右值，或者将亡值初始化另一个对象的时候，才会调用移动构造函数。而那个 move 语句，就是将一个左值变成一个将亡值

---

## 69、C++中将临时变量作为返回值时的处理过程

首先需要明白一件事情，**临时变量，在函数调用过程中是被压到程序进程的栈中的，当函数退出时，临时变量出栈，即临时变量已经被销毁，临时变量占用的内存空间没有被清空，但是可以被分配给其他变量**，所以有可能在函数退出时，该内存已经被修改了，对于临时变量来说已经是没有意义的值了

C语言里规定：16bit程序中，返回值保存在ax寄存器中，32bit程序中，返回值保持在eax寄存器中，如果是64bit返回值，edx寄存器保存高32bit，eax寄存器保存低32bit

由此可见，**函数调用结束后，返回值被临时存储到寄存器中，并没有放到堆或栈中，也就是说与内存没有关系了**。当退出函数的时候，临时变量可能被销毁，但是返回值却被放到寄存器中与临时变量的生命周期没有关系

如果我们需要返回值，一般使用赋值语句就可以了

---

## 70、如何获得结构成员相对于结构开头的字节偏移量

使用 <stddef.h> 头文件中的 offsetof 宏

```c++
#include <iostream>
#include <stddef.h>
using namespace std;

struct  S {
    int x;
    char y;
    int z;
    double a;
};

int main() {
    cout << offsetof(S, x) << endl; // 0
    cout << offsetof(S, y) << endl; // 4
    cout << offsetof(S, z) << endl; // 8
    cout << offsetof(S, a) << endl; // 16, 因为 double是 8 字节，需要找一个 8 的倍数对齐
    return 0;
}
```

```
+--------------------------------------------------------+
|   int x (4 bytes)   |   char y (1 byte)    |   Padding   |
+--------------------------------------------------------+
|   int z (4 bytes)   |    Padding          |    Padding   |
+--------------------------------------------------------+
|                  double a (8 bytes)                      |
+--------------------------------------------------------+

x 占用 4 个字节，位于偏移量 0 处。
y 占用 1 个字节，位于偏移量 4 处。
z 占用 4 个字节，位于偏移量 8 处。
a 占用 8 个字节，位于偏移量 16 处。
```


如果加上 `#pragma pack(4)` 指定 4 字节对齐方式 `offsetof(S, a) = 12`, 其余不变

```
+--------------------------------------------------------+
|   int x (4 bytes)   |   char y (1 byte)    |    Padding   |
+--------------------------------------------------------+
|   int z (4 bytes)   |   double a (8 bytes)               |
+--------------------------------------------------------+

x 占用 4 个字节，位于偏移量 0 处。y 占用 1 个字节，位于偏移量 4 处。
由于 y 后面的 z 是 4 字节对齐的，因此 y 后面会有 3 个字节的填充，位于偏移量 5~7 处。
z 占用 4 个字节，位于偏移量 8 处。
a 占用 8 个字节，位于偏移量 12 处。
由于结构体 S 中的 y 和 z 之间有 3 个字节的填充，保证 z 的地址是 4 的倍数。同时，结构体 S 的总大小是 20 字节，保证每个成员都按照 4 字节对齐。
```

```c++
// 也可以直接通过指针操作
cout << (long long)&((S *)nullptr)->a << endl; // 16
```

---

## 71、静态类型和动态类型，静态绑定和动态绑定的介绍

*   静态类型：对象在声明时采用的类型，在编译期既已确定；
*   动态类型：通常是指一个指针或引用目前所指对象的类型，是在运行期决定的；
*   静态绑定：绑定的是静态类型，所对应的函数或属性依赖于对象的静态类型，发生在编译期；
*   动态绑定：绑定的是动态类型，所对应的函数或属性依赖于对象的动态类型，发生在运行期；

从上面的定义也可以看出，非虚函数一般都是静态绑定，而虚函数都是动态绑定（如此才可实现多态性）

```c++
class A {
public:
    /*virtual*/ void func() { std::cout << "A::func()\n"; }
};

class B : public A {
public:
    void func() { std::cout << "B::func()\n"; }
};

class C : public A {
public:
    void func() { std::cout << "C::func()\n"; }
};

int main() {
    C* pc = new C(); // pc 静态类型是它声明的类型C*，动态类型也是C*
    B* pb = new B(); // pb 静态类型 C*, 动态类型 B*
    A* pa = pc;      // pa 静态类型 A*, 动态类型 C*
    pa = pb;         // pa 静态类型 A*, 动态类型 B* (动态类型可以更改)
    C *pnull = nullptr; // pnull 静态类型 C*, 没有动态类型，因为它指向了 nullptr

    pa->func();      // A::func() pa 的静态类型永远都是A*, 不管其指向的是哪个子类，都是直接调用A::func()；
    pc->func();      // C::func() pc 的动、静态类型都是C*，因此调用C::func()；
    pnull->func();   // C::func() 不用奇怪为什么空指针也可以调用函数，因为这在编译期就确定了，和指针空不空没关系；
    return 0;
}
```

如果将 A 类中的 `virtual` 注释去掉，则运行结果是

```c++
pa->func();      // B::func() 因为有了virtual虚函数特性，pa的动态类型指向B*，因此先在B中查找，找到后直接调用；
pc->func();      // C::func() pc的动、静态类型都是C*，因此也是先在C中查找；
pnull->func();   // 空指针异常，因为是 func 是 virtual 函数，因此对 func 的调用只能等到运行期才能确定，然后才发现 pnull 是空指针；
```

*   如果基类A中的 func **不是虚函数**，那么不论pa、pb、pc指向哪个子类对象，对func的调用都是在定义pa、pb、pc时的**静态类型决定，早已在编译期确定了**
    
*   同样的**空指针也能够直接调用no-virtual函数而不报错**（这也说明一定要做空指针检查啊！），因此静态绑定不能实现多态；
    
*   **如果func是虚函数，那所有的调用都要等到运行时根据其指向对象的类型才能确定**，比起静态绑定自然是要有性能损失的，但是却能实现多态特性；
    
**引用同理**

至此总结一下静态绑定和动态绑定的区别：

*   静态绑定发生在编译期，动态绑定发生在运行期；
    
*   对象的动态类型可以更改，但是静态类型无法更改；
    
*   要想实现多态，必须使用动态绑定；
    
*   在继承体系中只有虚函数使用的是动态绑定，其他的全部是静态绑定；
    

**建议：**

绝对不要重新定义继承而来的非虚(non-virtual)函数（《Effective C++ 第三版》条款36），因为这样导致函数调用由对象声明时的静态类型确定了，而和对象本身脱离了关系，没有多态，也这将给程序留下不可预知的隐患和莫名其妙的BUG；另外，在动态绑定也即在virtual函数中，要注意默认参数的使用。**当缺省参数和virtual函数一起使用的时候一定要谨慎，不然出了问题怕是很难排查**。 看下面的代码：

```c++
class Base {
public:
    virtual void func (int i = 0) {
        std::cout << "Base::func()\t" << i << "\n";
    }
};

class Son : public Base {
public:
    virtual void func (int i = 1) {
        std::cout << "Son::func()\t" << i << "\n";
    }
};

int main() {
    Son* pSon = new Son();
    Base* pBase = pSon;
    pSon->func(); //Son::func() 1  
    pBase->func(); //Son::func() 0  !!!! 调用了子类的函数，却使用了基类中参数的默认值！
    return 0;
}
```

**这是因为默认参数的绑定发生在编译时期，而虚函数的调用发生在运行时期**。因为虚函数是动态绑定的, `pBase` 的动态类型为 `Son*`, 所以会调用 `Son::func()` 然而函数的默认参数值在编译时期就已经确定了, 此时编译器只知道 `pBase` 的静态类型为 `Base*` , 无法知道其指向的是否为 `Son` 对象。因此，在编译时期，编译器会根据 `pBase` 的类型 `Base*` 来确定默认参数的值, 而不是根据运行时的动态类型来确定。所以，`pBase->func()` 中默认参数的值是 `Base::func()` 中的默认值，即 0

**虚函数调用会根据对象的动态类型来确定调用的函数，但是默认参数值在编译时期就已经确定了，不会根据运行时期对象的类型而变化**

---

## 72、引用是否能实现动态绑定，为什么可以实现？

**可以, 但注意引用在创建的时候必须初始化，在访问虚函数时，编译器会根据其所绑定的对象类型决定要调用哪个函数。注意只能调用虚函数**

```c++
class Base {
public:
    virtual void func () {
        std::cout << "Base::func()\n";
    }
};

class Son : public Base {
public:
    void func () override {
        std::cout << "Son::func()\n";
    }
    void sonFunc() {
        std::cout << "Son::sonFunc()\n";
    }
};

int main() {
    Son s;
    Base& base = s;
    base.func(); //Son::func()
    base.sonFunc(); // error: 'class Base' has no member named 'sonFunc'
    return 0;
}
```

**需要说明的是虚函数才具有动态绑定**，所以 `base` 不能调用 `Son` 中的非虚函数 `sonFunc()`。如果使用基类指针来指向子类也是一样的。

---

## 73、全局变量和局部变量有什么区别？

生命周期不同：全局变量随主程序创建而创建，随主程序销毁而销毁；局部变量在局部函数内部，甚至局部循环体等内部存在，退出就不存在；

使用方式不同：通过声明后全局变量在程序的各个部分都可以用到；局部变量分配在堆栈区，只能在局部使用

操作系统和编译器通过内存分配的位置可以区分两者，全局变量分配在全局数据段并且在程序开始运行的时候被加载。局部变量则分配在堆栈里面

---

## 74、指针加减计算要注意什么？

**指针加减本质是对其所指地址的移动，移动的步长跟指针的类型是有关系的**，因此在涉及到指针加减运算需要十分小心，加多或者减多都会导致指针指向一块未知的内存地址，如果再进行操作就会很危险。

```c++
int main() {
    int *a, *b, c;
    a = (int*)0x500;
    b = (int*)0x520;
    c = b - a;
    printf("%d\n", c); // 8
    a += 0x020;
    c = b - a;
    printf("%d\n", c); // -24
    return 0;
}
```

首先变量 a 和 b 都是以16进制的形式初始化，将它们转成10进制分别是 `1280（5*16^2=1280）`和`1312（5*16^2+2*16=1312)`， 那么它们的差值为 32，也就是说 a 和 b 所指向的地址之间间隔 32 个位，但是考虑到是 `int` 类型占4位，所以 c 的值为 32/4=8

a 自增16进制 `0x20` 之后，其实际地址变为 `1280 + 2*16*4 = 1408`，（因为一个int占4位，所以要乘4），这样它们的差值就变成了 `1280 - 1312 = -96`，所以 c 的值就变成了`-96/4 = -24`

遇到指针的计算，**需要明确的是指针每移动一位，它实际跨越的内存间隔是指针类型的长度，建议都转成10进制计算，计算结果除以类型长度取得结果**

---

## 75、 怎样判断两个浮点数是否相等？

对两个浮点数判断大小和是否相等不能直接用 == 来判断，会出错！明明相等的两个数比较反而是不相等！**对于两个浮点数比较只能通过相减并与预先设定的精度比较，记得要取绝对值！浮点数与0的比较也应该注意。与浮点数的表示方式有关**

---

## 76、方法调用的原理（栈，汇编）

**函数调用的原理：**

1. 函数调用时，首先需要将参数传递给被调用函数。参数传递的方式可以是通过寄存器传递，也可以通过栈传递。

2. 在函数调用前，需要将函数的返回地址保存起来，以便在函数执行完毕后返回到正确的调用位置。这个返回地址也会被保存在栈中。

3. 在函数调用过程中，被调用函数会在栈上创建一个新的栈帧，用来保存函数的局部变量、临时数据和其他需要保存的信息。

4. 在函数执行过程中，可能会调用其他函数，此时会依次创建新的栈帧并将函数的参数和返回地址压入栈中。

5. 当函数执行完毕后，会从栈中弹出当前函数的栈帧，并将返回地址恢复，然后跳转回调用者的位置继续执行。

帧栈可以认为是程序栈的一段，它有两个端点，一个标识起始地址，一个标识着结束地址，两个指针结束地址指针esp，开始地址指针ebp; 每一个栈指针+4的位置存储函数返回地址；每一个栈帧都建立在调用者的下方，当被调用者执行完毕时，这一段栈帧会被释放。由于栈帧是向地址递减的方向延伸，因此如果我们将栈指针减去一定的值，就相当于给栈帧分配了一定空间的内存。如果将栈指针加上一定的值，也就是向上移动，那么就相当于压缩了栈帧的长度，也就是说内存被释放了

**栈帧的构造过程：**

1. 首先，被调用函数会保存调用者保存的寄存器状态，以防止在函数执行过程中被修改。

2. 然后，函数会为局部变量和临时数据在栈上分配空间，并将函数的参数和局部变量保存在栈帧中。

3. 如果函数调用其他函数，会依次创建新的栈帧，并将参数和返回地址压入栈中。

**栈帧的销毁过程：**

1. 当函数执行完毕后，会从栈中弹出当前函数的栈帧，同时恢复调用者保存的寄存器状态。

2. 如果函数调用了其他函数，会依次销毁这些函数的栈帧。


**过程实现**
    
① 备份原来的帧指针，调整当前的栈帧指针到栈指针位置；

② 建立起来的栈帧就是为被调用者准备的，当被调用者使用栈帧时，需要给临时变量分配预留内存；

③ 使用建立好的栈帧，比如读取和写入，一般使用mov，push以及pop指令等等。

④ 恢复被调用者寄存器当中的值，这一过程其实是从栈帧中将备份的值再恢复到寄存器，不过此时这些值可能已经不在栈顶了

⑤ 释放被调用者的栈帧，释放就意味着将栈指针加大，而具体的做法一般是直接将栈指针指向帧指针，因此会采用类似下面的汇编代码处理。

⑥ 恢复调用者的栈帧，恢复其实就是调整栈帧两端，使得当前栈帧的区域又回到了原始的位置。

⑦ 弹出返回地址，跳出当前过程，继续执行调用者的代码。

**过程调用和返回指令**

① call指令

② leave指令

③ ret指令

---​

## 77、C++中的指针参数传递和引用参数传递有什么区别？底层原理你知道吗？

- **指针参数传递本质上是值传递，它所传递的是一个地址值**

值传递过程中，被调函数的形式参数作为被调函数的局部变量处理，会在栈中开辟内存空间以存放由主调函数传递进来的实参值，从而形成了实参的一个副本（替身）。

**值传递的特点是，被调函数对形式参数的任何操作都是作为局部变量进行的，不会影响主调函数的实参变量的值**（形参指针变了，实参指针不会变）

- **引用参数传递过程中，被调函数的形式参数也作为局部变量在栈中开辟了内存空间，但是这时存放的是由主调函数放进来的实参变量的地址**

**被调函数对形参（本体）的任何操作都被处理成间接寻址**，即通过栈中存放的地址访问主调函数中的实参变量（根据别名找到主调函数中的本体）

因此，被调函数对形参的任何操作都会影响主调函数中的实参变量。

- **引用传递和指针传递是不同的，虽然他们都是在被调函数栈空间上的一个局部变量，但是任何对于引用参数的处理都会通过一个间接寻址的方式操作到主调函数中的相关变量**

而对于指针传递的参数，如果改变被调函数中的指针地址，它将应用不到主调函数的相关变量。如果想通过指针参数传递来改变主调函数中的相关变量（地址），那就得使用指向指针的指针或者指针引用。

- **从编译的角度来讲，程序在编译时分别将指针和引用添加到符号表上，符号表中记录的是变量名及变量所对应地址**

指针变量在符号表上对应的地址值为指针变量的地址值，而引用在符号表上对应的地址值为引用对象的地址值（与实参名字不同，地址相同）

符号表生成之后就不会再改，因此指针可以改变其指向的对象（指针变量中的值可以改），而引用对象则不能修改

---

## 78、类如何实现只能静态分配和只能动态分配

1.  前者是把 new、delete 运算符重载为 private 属性。后者是把构造、析构函数设为 protected 属性，再用子类来动态创建
    
2.  建立类的对象有两种方式：
    
① 静态建立，静态建立一个类对象，就是由编译器为对象在栈空间中分配内存；

② 动态建立，`A *p = new A();` 动态建立一个类对象，就是使用 new 运算符为对象在堆空间中分配内存。这个过程分为两步，第一步执行 `operator new()` 函数，在堆中搜索一块内存并进行分配；第二步调用类构造函数构造对象；

3.  只有使用new运算符，对象才会被建立在堆上，因此只要限制new运算符就可以实现类对象只能建立在栈上，可以将new运算符设为私有

---

## 79、如果想将某个类用作基类，为什么该类必须定义而非声明？

派生类中包含并且可以使用它从基类继承而来的成员，为了使用这些成员，派生类必须知道他们是什么。

所以必须定义而非声明

---

## 80、 继承机制中对象之间如何转换？指针和引用之间如何转换？

1. 向上类型转换
        
将派生类指针或引用转换为基类的指针或引用被称为向上类型转换，向上类型转换会自动进行，而且**向上类型转换是安全的**

2. 向下类型转换
            
将基类指针或引用转换为派生类指针或引用被称为向下类型转换，向下类型转换不会自动进行，因为一个基类对应几个派生类，所以向下类型转换时不知道对应哪个派生类，所以**在向下类型转换时必须加动态类型识别技术。RTTI 技术，用 dynamic_cast 进行向下类型转换**