**C++之STL模板库**

- [1、什么是STL？](#1什么是stl)
- [2、解释一下什么是 trivial destructor](#2解释一下什么是-trivial-destructor)
- [3、使用智能指针管理内存资源，RAII 是怎么回事？](#3使用智能指针管理内存资源raii-是怎么回事)
- [4、迭代器：++it、it++哪个好，为什么](#4迭代器itit哪个好为什么)
- [5、说一下C++左值引用和右值引用](#5说一下c左值引用和右值引用)
  - [左值和右值](#左值和右值)
  - [左值引用和右值引用](#左值引用和右值引用)
  - [右值引用的特点](#右值引用的特点)
- [6、STL中 hashtable 的实现？](#6stl中-hashtable-的实现)
- [7、简单说一下 traits 技法](#7简单说一下-traits-技法)
- [8、STL的两级空间配置器](#8stl的两级空间配置器)
  - [一级配置器](#一级配置器)
  - [二级配置器](#二级配置器)
- [9、 vector与list的区别与应用？怎么找某vector或者list的倒数第二个元素](#9-vector与list的区别与应用怎么找某vector或者list的倒数第二个元素)
- [10、STL 中vector删除其中的元素，迭代器如何变化？为什么是两倍扩容？释放空间？](#10stl-中vector删除其中的元素迭代器如何变化为什么是两倍扩容释放空间)
- [11、Vector如何释放空间?](#11vector如何释放空间)
- [12、容器内部删除一个元素](#12容器内部删除一个元素)
- [13、STL迭代器如何实现](#13stl迭代器如何实现)
- [14、map、set是怎么实现的，红黑树是怎么能够同时实现这两种容器？ 为什么使用红黑树？](#14mapset是怎么实现的红黑树是怎么能够同时实现这两种容器-为什么使用红黑树)
- [15、如何在共享内存上使用STL标准库？](#15如何在共享内存上使用stl标准库)
- [16、map插入方式有哪几种？](#16map插入方式有哪几种)
- [17、STL中unordered\_map(hash\_map)和map的区别，hash\_map如何解决冲突以及扩容](#17stl中unordered_maphash_map和map的区别hash_map如何解决冲突以及扩容)
- [18、vector越界访问下标，map越界访问下标？vector删除元素时会不会释放空间？](#18vector越界访问下标map越界访问下标vector删除元素时会不会释放空间)
- [19、map中 \[\] 与 find 的区别？](#19map中--与-find-的区别)
- [20、 STL中 list 和 queue 与 vector 之间的区别](#20-stl中-list-和-queue-与-vector-之间的区别)
- [21、STL中的allocator、deallocator](#21stl中的allocatordeallocator)
- [22、STL中hash table扩容发生什么？](#22stl中hash-table扩容发生什么)
- [23、常见容器性质总结？](#23常见容器性质总结)
- [24、vector的增加删除都是怎么做的？为什么是1.5或者是2倍？](#24vector的增加删除都是怎么做的为什么是15或者是2倍)
- [25、说一下STL每种容器对应的迭代器](#25说一下stl每种容器对应的迭代器)
- [26、STL中迭代器失效的情况有哪些？](#26stl中迭代器失效的情况有哪些)
- [27、STL中vector的实现](#27stl中vector的实现)
- [28、STL中slist的实现](#28stl中slist的实现)
- [29、STL中list的实现](#29stl中list的实现)
- [30、STL中的deque的实现](#30stl中的deque的实现)
- [31、STL中stack和queue的实现](#31stl中stack和queue的实现)
- [32、STL中的 heap 的实现](#32stl中的-heap-的实现)
- [33、STL中的 priority\_queue 的实现](#33stl中的-priority_queue-的实现)
- [34、STL中set的实现？](#34stl中set的实现)
- [35、STL中 map 的实现](#35stl中-map-的实现)
- [36、set 和 map 的区别，multimap 和 multiset 的区别](#36set-和-map-的区别multimap-和-multiset-的区别)
- [37、STL中 unordered\_map 和 map 的区别和应用场景](#37stl中-unordered_map-和-map-的区别和应用场景)
- [38、hashtable中解决冲突有哪些方法？](#38hashtable中解决冲突有哪些方法)

## 1、什么是STL？

C++ STL从广义来讲包括了三类：算法，容器和迭代器

*   算法包括排序，复制等常用算法，以及不同容器特定的算法。
*   容器就是数据的存放形式，包括序列式容器和关联式容器，序列式容器就是 list，vector 等，关联式容器就是 set，map 等
*   迭代器就是在不暴露容器内部结构的情况下对容器的遍历

---

## 2、解释一下什么是 trivial destructor

trivial destructor 一般是指用户没有自定义析构函数，而由系统生成的，这种析构函数在《STL源码解析》中称为 “无关痛痒” 的析构函数

反之，用户自定义了析构函数，则称之为 “non-trivial destructor”，这种析构函数**如果申请了新的空间一定要显式的释放，否则会造成内存泄露**

对于trivial destructor，如果每次都进行调用，显然对效率是一种伤害，如何进行判断呢？

《STL源码解析》中给出的说明是:

首先利用 `value_type()` 获取所指对象的类别，再利用 `__type_traits<T>` 判断该型别的析构函数是否`trivial`，若是 (`__true_type`)，则什么也不做，若为 (`__false_type`)，则去调用 `destory()` 函数

除了 trivial destructor，还有 trivial construct、trivial copy construct 等，如果能够对是否trivial进行区分，可以采用内存处理函数 memcpy()、malloc() 等更加高效的完成相关操作，提升效率

---

## 3、使用智能指针管理内存资源，RAII 是怎么回事？

1.  RAII 全称是 “Resource Acquisition is Initialization”，直译过来是“资源获取即初始化”，也就是说在构造函数中申请分配资源，在析构函数中释放资源。

因为C++的语言机制保证了，当一个对象创建的时候，自动调用构造函数，当对象超出作用域的时候会自动调用析构函数。所以，在 RAII 的指导下，我们应该使用类来管理资源，将资源和对象的生命周期绑定。

2.  智能指针（`std::shared_ptr, std::unique_ptr`）即 RAII 最具代表的实现，使用智能指针，可以实现自动的内存管理，再也不需要担心忘记 delete 造成的内存泄漏

毫不夸张的来讲，有了智能指针，代码中几乎不需要再出现delete了

---

## 4、迭代器：++it、it++哪个好，为什么

- 前置返回一个引用，后置返回一个对象

```c++
// ++i 实现代码为：
int& operator++() {
    *this += 1;
    return *this;
} 
```

- 前置不会产生临时对象，后置必须产生临时对象，临时对象会导致效率降低

```c++
// i++ 实现代码为：                 
int operator++(int) {
    int temp = *this;                   
    ++*this;                       
    return temp;                  
} 
```
---

## 5、说一下C++左值引用和右值引用

C++11引入了右值引用和移动语义的特性，主要为了优化性能并解决拷贝操作带来的性能损失。具体来说，**移动语义允许我们在不引入额外开销的情况下将资源（如动态分配的内存、文件句柄等）从一个对象转移到另一个对象，从而避免了深拷贝的开销，提高了程序的性能；右值引用可以识别临时对象（右值），并将其资源转移到另一个对象，避免不必要的拷贝；完美转发（perfect forwarding）解决了参数转发中类型丢失的问题，使得我们可以按照参数实际类型将参数转发给其他函数** （完美转发通过使用通用引用（即右值引用结合模板推导）来实现，获得的一个好处是可以实现移动语义）

1.  在C++11中所有的值必属于左值、右值两者之一，右值又可以细分为纯右值、将亡值。在C++11中可**以取地址的、有名字的就是左值，反之，不能取地址的、没有名字的就是右值**（将亡值或纯右值）。举个例子，int a = b+c, a 就是左值，其有变量名为a，通过&a可以获取该变量的地址；表达式b+c、函数int func()的返回值是右值，在其被赋值给某一变量前，我们不能通过变量名找到它，＆(b+c)这样的操作则不会通过编译。
    
2.  C++11对C++98中的右值进行了扩充。在C++11中右值又分为纯右值（prvalue，Pure Rvalue）和将亡值（xvalue，eXpiring Value）。其中纯右值的概念等同于我们在C++98标准中右值的概念，指的是临时变量和不跟对象关联的字面量值；将亡值则是C++11新增的跟右值引用相关的表达式，这样表达式通常是将要被移动的对象（移为他用），比如返回右值引用T&&的函数返回值、std::move的返回值，或者转换为T&&的类型转换函数的返回值。**将亡值可以理解为通过“盗取”其他变量内存空间的方式获取到的值。在确保其他变量不再被使用、或即将被销毁时，通过“盗取”的方式可以避免内存空间的释放和分配，能够延长变量值的生命期**
    
3.  左值引用就是对一个左值进行引用的类型。右值引用就是对一个右值进行引用的类型，事实上，由于右值通常不具有名字，我们也只能通过引用的方式找到它的存在。右值引用和左值引用都是属于引用类型。无论是声明一个左值引用还是右值引用，都必须立即进行初始化。而其原因可以理解为是引用类型本身自己并不拥有所绑定对象的内存，只是该对象的一个别名。左值引用是具名变量值的别名，而右值引用则是不具名（匿名）变量的别名。左值引用通常也不能绑定到右值，但**常量左值引用是个“万能”的引用类型**。它可以接受非常量左值、常量左值、右值对其进行初始化。不过常量左值所引用的右值在它的“余生”中只能是只读的。相对地，非常量左值只能接受非常量左值对其进行初始化。
    
4.  右值值引用通常不能绑定到任何的左值，**要想绑定一个左值到右值引用，通常需要 `std::move()` 将左值强制转换为右值**
    

### 左值和右值

左值：表示的是可以获取地址的表达式，它能出现在赋值语句的左边，对该表达式进行赋值。但是修饰符const的出现使得可以声明如下的标识符，它可以取得地址，但是没办法对其进行赋值 (`const int& a = 10;`)

右值：**表示无法获取地址的对象，有常量值、函数返回值、lambda表达式等。无法获取地址，但不表示其不可改变，当定义了右值的右值引用时就可以更改右值**

### 左值引用和右值引用

左值引用：传统的C++中引用被称为左值引用

右值引用：C++11中增加了右值引用，**右值引用关联到右值时，右值被存储到特定位置，右值引用指向该特定位置，也就是说，右值虽然无法获取地址，但是右值引用是可以获取地址的，该地址表示临时对象的存储位置**

### 右值引用的特点

*   特点1：**通过右值引用的声明，右值又“重获新生”，其生命周期与右值引用类型变量的生命周期一样长**，只要该变量还活着，该右值临时量将会一直存活下去
*   特点2：右值引用独立于左值和右值。意思是右值引用类型的变量可能是左值也可能是右值
*   特点3：**T&& t 在发生自动类型推断的时候，它是左值还是右值取决于它的初始化**

```c++
#include <bits/stdc++.h>
using namespace std;

template<typename T>
void fun(T&& t) {
    cout << t << endl;
}

int getInt() {
    return 5;
}

int main() {
    int a = 10;
    int& b = a;  
    int& c = 10;    // 错误，c是左值不能使用右值初始化
    int&& d = 10; 
    int&& e = a;    // 错误，e是右值引用不能使用左值初始化
    const int& f = a;       // 正确，左值常引用相当于是万能型，可以用左值或者右值初始化
    const int& g = 10;      // 正确，左值常引用相当于是万能型，可以用左值或者右值初始化
    const int&& h = 10;     // 正确，右值常引用
    const int& aa = h;      // 正确
    int& i = getInt();      // 错误，i是左值引用不能使用临时变量（右值）初始化
    int&& j = getInt();  
    fun(10);    // 此时fun函数的参数t是右值
    fun(a);     // 此时fun函数的参数t是左值
    return 0;
}
```
---

## 6、STL中 hashtable 的实现？

STL中的 hashtable 使用的是**链地址法(开链法)**解决hash冲突问题，如下图所示。

![](http://oss.interviewguide.cn/img/202205220035271.png)

hashtable中的bucket所维护的list既不是list也不是slist，而是其自己定义的由 `hashtable_node` 数据结构组成的 `linked-list`，而bucket聚合体本身使用vector进行存储。**hashtable 的迭代器只提供前进操作，不提供后退操作**

在hashtable设计bucket的数量上，其内置了28个质数 `[53, 97, 193,...,429496729]`，在创建hashtable时，会根据存入的元素个数选择大于等于元素个数的质数作为hashtable的容量（vector的长度），其中每个bucket所维护的linked-list长度也等于hashtable的容量。如果插入hashtable的元素个数超过了bucket的容量，就要进行重建table操作，即找出下一个质数，创建新的buckets vector，重新计算元素在新hashtable的位置

总结起来，STL 中 hashtable 的设计是为了提供高效的哈希表实现，使用了质数作为 bucket 的数量，采用开链法解决哈希冲突，并使用 vector 存储 buckets。同时，迭代器只提供前进操作，并且在元素数量过多时会进行重建 table 的操作，确保 hashtable 的高效性能

---

## 7、简单说一下 traits 技法

traits 技法是一种强大的编程技巧，**利用了 C++ 的模板和类型推导功能来获取类型信息并增强类型的认证能力**。在 C++ 中，我们经常使用 `iterator_traits` 和 `type_traits` 来获取和判断类型信息，使得代码更加灵活和通用

**iterator_traits**

`iterator_traits` 提供了一种统一的方式来获取迭代器的相关类型信息，如迭代器指向的值类型、迭代器之间的距离类型、指针类型等。通过使用 `iterator_traits`，我们可以在泛型代码中使用迭代器而不用关心其具体类型，从而增强了代码的通用性和复用性

*   value_type：迭代器所指对象的类型
*   reference：迭代器所引用的类型
*   pointer：迭代器所指向的类型
*   difference_type：两个迭代器之间的距离类型
*   iterator_category：

```c++
template <typename Iterator>
void print_iterator_info(Iterator it) {
    using Traits = iterator_traits<Iterator>;
    cout << "值类型: " << typeid(typename Traits::value_type).name() << endl;
    cout << "引用类型: " << typeid(typename Traits::reference).name() << endl;
    cout << "指针类型: " << typeid(typename Traits::pointer).name() << endl;
    cout << "差值类型: " << typeid(typename Traits::difference_type).name() << endl;
    cout << "迭代器类别: " << typeid(typename Traits::iterator_category).name() << endl;
}

int main() {
    vector<int> vec = {1, 2, 3, 4, 5};
    auto it = vec.begin();
    print_iterator_info(it);
    return 0;
}

// 值类型: i
// 引用类型: i
// 指针类型: Pi
// 差值类型: x
// 迭代器类别: St26random_access_iterator_tag
```

**type_traits**

`type_traits` 则提供了一系列的模板类和函数，用于在编译时判断和获取类型的特性。例如，std::is_pointer 可以判断一个类型是否是指针类型，std::remove_reference 可以去除引用修饰等。这些特性判断和类型转换可以在编译时完成，避免了运行时的开销，同时也让代码更加健壮和安全

`type_traits` 还可以判断类型的**特性**，包括是否具备默认构造函数、拷贝构造函数、赋值运算符和析构函数等。这些信息对于编写通用代码和进行优化都非常重要，一般来说，`type_traits` 支持以下5中类型的判断：

- std::is_default_constructible：判断类型是否具有默认构造函数。
- std::is_copy_constructible：判断类型是否具有拷贝构造函数。
- std::is_copy_assignable：判断类型是否具有赋值运算符。
- std::is_move_constructible：判断类型是否具有移动构造函数。
- std::is_move_assignable：判断类型是否具有移动赋值运算符

这些 type_traits 可以在编译时判断类型的特性，以便在代码中进行相应的处理。比如，当类型具有特定特性时，可以采取更高效的操作，提高代码的性能。例如，对于不具备拷贝构造函数的类型，可以采用直接操作内存的方式来进行复制，从而避免调用拷贝构造函数，提高效率

```c++
#include <iostream>
#include <type_traits>
using namespace std;

template<typename T>
void PrintTypeInfo() {
    std::cout << "Type: " << typeid(T).name() << std::endl;
    std::cout << "is_default_constructible: " << std::is_default_constructible<T>::value << std::endl;
    std::cout << "is_copy_constructible: " << std::is_copy_constructible<T>::value << std::endl;
    std::cout << "is_copy_assignable: " << std::is_copy_assignable<T>::value << std::endl;
    std::cout << "is_move_constructible: " << std::is_move_constructible<T>::value << std::endl;
    std::cout << "is_move_assignable: " << std::is_move_assignable<T>::value << std::endl;
}

// 类A 具有所有特性 (编译器生成)：默认构造函数、拷贝构造函数、赋值运算符、移动构造函数和移动赋值运算符
class A {
public:
    A() {}
};

// 类B 只具有默认构造函数和拷贝构造函数 (编译器自动生成其余函数)
class B {
public:
    B() {}
    B(const B&) {}
};

// 类C 显式提供了移动构造函数时，编译器会认为你对类的移动语义有特殊需求，因此不会再为你自动生成
// 默认的拷贝构造函数，拷贝赋值函数和移动赋值函数
class C {
public:
    C() {}
    C(C &&) {}
};


int main() {
    // 使用type_traits来检查类型的属性
    PrintTypeInfo<A>();
    PrintTypeInfo<B>();
    PrintTypeInfo<C>();
    return 0;
}

// Type: 1A
// is_default_constructible: 1
// is_copy_constructible: 1
// is_copy_assignable: 1
// is_move_constructible: 1
// is_move_assignable: 1

// Type: 1B
// is_default_constructible: 1
// is_copy_constructible: 1
// is_copy_assignable: 1
// is_move_constructible: 1
// is_move_assignable: 1

// Type: 1C
// is_default_constructible: 1
// is_copy_constructible: 0
// is_copy_assignable: 0
// is_move_constructible: 1
// is_move_assignable: 0
```

---

## 8、STL的两级空间配置器

1、首先明白为什么需要二级空间配置器？

我们知道动态开辟内存时，要在堆上申请，但若是我们需要

频繁的在堆开辟释放内存，则就会**在堆上造成很多外部碎片**，浪费了内存空间；(分散在已分配内存块之间的未使用的小块内存)

每次都要进行调用**malloc、free**函数等操作，使空间就会增加一些附加信息，降低了空间利用率；

随着外部碎片增多，内存分配器在找不到合适内存情况下需要合并空闲块，浪费了时间，大大降低了效率。

于是就设置了二级空间配置器，**当开辟内存<=128bytes时，即视为开辟小块内存，则调用二级空间配置器。**

关于STL中一级空间配置器和二级空间配置器的选择上，一般默认**选择的为二级空间配置器**。 如果大于128字节再转去一级配置器器。

### 一级配置器

**一级空间配置器**中重要的函数就是 `allocate、deallocate、reallocate`。 一级空间配置器是以`malloc()，free()，realloc()` 等C函数执行实际的内存配置 。大致过程是：

1、直接allocate分配内存，其实就是malloc来分配内存，成功则直接返回，失败就调用处理函数

2、如果用户自定义了内存分配失败的处理函数就调用，没有的话就返回异常

3、如果自定义了处理函数就进行处理，完事再继续分配试试

![](http://oss.interviewguide.cn/img/202205220035143.png)

### 二级配置器

![](http://oss.interviewguide.cn/img/202205220035104.png)

1、维护16条链表，分别是0-15号链表，最小8字节，以8字节逐渐递增，最大128字节，你传入一个字节参数，表示你需要多大的内存，会自动帮你校对到第几号链表（如需要13bytes空间，我们会给它分配16bytes大小），在找到第n个链表后查看链表是否为空，如果不为空直接从对应的 free_list 中拔出，将已经拨出的指针向后移动一位。

2、对应的free_list为空，先看其内存池是不是空时，如果内存池不为空： 
（1）先检验它剩余空间是否够20个节点大小（即所需内存大小(提升后) * 20），若足够则直接从内存池中拿出20个节点大小空间，将其中一个分配给用户使用，另外19个当作自由链表中的区块挂在相应的free_list下，这样下次再有相同大小的内存需求时，可直接拨出。 
（2）如果不够20个节点大小，则看它是否能满足1个节点大小，如果够的话则直接拿出一个分配给用户，然后从剩余的空间中分配尽可能多的节点挂在相应的free_list中。 
（3）如果连一个节点内存都不能满足的话，则将内存池中剩余的空间挂在相应的free_list中（找到相应的free_list），然后再给内存池申请内存，转到3。 

3、内存池为空，申请内存 此时二级空间配置器会使用 malloc() 从heap上申请内存，（一次所申请的内存大小为2 * 所需节点内存大小（提升后）* 20 + 一段额外空间），申请40块，一半拿来用，一半放内存池中。 

4、malloc没有成功 在第三种情况下，如果malloc()失败了，说明heap上没有足够空间分配给我们了，这时，二级空间配置器会从比所需节点空间大的free_list中一一搜索，从比它所需节点空间大的free_list中拔除一个节点来使用。如果这也没找到，说明比其大的free_list中都没有自由区块了，那就要调用一级适配器了

释放时调用deallocate()函数，若释放的n>128，则调用一级空间配置器，否则就直接将内存块挂上自由链表的合适位置。

STL二级空间配置器虽然解决了外部碎片与提高了效率，但它同时增加了一些缺点：

1.因为自由链表的管理问题，它会把我们需求的内存块自动提升为8的倍数，这时若你需要1个字节，它会给你8个字节，即浪费了7个字节，所以它又引入了内部碎片的问题，若相似情况出现很多次，就会造成很多内部碎片；

2.二级空间配置器是在堆上申请大块的狭义内存池，然后用自由链表管理，供现在使用，在程序执行过程中，它将申请的内存一块一块都挂在自由链表上，即不会还给操作系统，并且它的实现中所有成员全是静态的，所以它申请的所有内存只有在进程结束才会释放内存，还给操作系统，由此带来的问题有：1.即我不断的开辟小块内存，最后整个堆上的空间都被挂在自由链表上，若我想开辟大块内存就会失败；2.若自由链表上挂很多内存块没有被使用，当前进程又占着内存不释放，这时别的进程在堆上申请不到空间，也不可以使用当前进程的空闲内存，由此就会引发多种问题

---

## 9、 vector与list的区别与应用？怎么找某vector或者list的倒数第二个元素

**vector**

- 随机访问高效（时间复杂度为O(1)）。
- 插入和删除（不包括尾部）需要移动数据，操作相对复杂和耗时。
- 迭代器是单向的，只能进行正向遍历。
- 迭代器在使用后可能会失效，因为插入和删除可能会导致动态数组的重新分配。

**list**

- 随机访问低效（时间复杂度为O(n)），需要遍历整个链表。
- 插入和删除操作非常高效（时间复杂度为O(1)），只需改变指针的指向。
- 迭代器是双向的，支持正向和反向遍历。
- 迭代器在使用后仍然有效，因为插入和删除不影响其他节点的指针。
- ist作为双向链表，每个节点都需要额外的指针来维护链表结构，因此相比于连续存储的容器，list的空间开销较大

所以vector可以直接利用下标找倒数第二个元素，而list可以从尾部迭代器开始往前移动两位找到倒数第二个元素

---

## 10、STL 中vector删除其中的元素，迭代器如何变化？为什么是两倍扩容？释放空间？

size()函数返回的是已用空间大小，capacity()返回的是总空间大小，capacity()-size()则是剩余的可用空间大小。当size()和capacity()相等，说明vector目前的空间已被用完，如果再添加新元素，则会引起vector空间的动态增长。

由于动态增长会引起重新分配内存空间、拷贝原空间、释放原空间，这些过程会降低程序效率。因此，可以使用reserve(n)预先分配一块较大的指定大小的内存空间，这样当指定大小的内存空间未使用完时，是不会重新分配内存空间的，这样便提升了效率。只有当n>capacity()时，调用reserve(n)才会改变vector容量。

resize()成员函数改变元素的数目，至于空间的的变化需要看具体情况去分析，如下：

```c++
void resize(size_type __new_size, const _Tp& __x) {
    if (__new_size < size()) 
          erase(begin() + __new_size, end());
    else
          insert(end(), __new_size - size(), __x);
}
```

1、空的vector对象，size()和capacity()都为 0

2、当空间大小不足时，新分配的空间大小为原空间大小的2倍。

3、使用reserve()预先分配一块内存后，在空间未满的情况下，不会引起重新分配，从而提升了效率。

4、**当reserve()分配的空间比原空间小时，是不会引起重新分配的**

5、**resize()函数只改变容器的元素数目，未改变容器大小**

6、**用reserve(size_type)只是扩大capacity值，这些内存空间可能还是“野”的，如果此时使用 [ ] 来访问，则可能会越界。而resize(size_type new_size)会真正使容器具有new_size个对象**

不同的编译器，vector有不同的扩容大小。在vs下是1.5倍，在GCC下是2倍；

空间和时间的权衡。简单来说， 空间分配的多，平摊时间复杂度低，但浪费空间也多

使用k=2增长因子的问题在于，每次扩展的新尺寸必然刚好大于之前分配的总和，也就是说，之前分配的内存空间不可能被使用。这样对内存不友好，最好把增长因子设为(1, 2)，也就是1-2之间的某个数值。

对比可以发现采用采用成倍方式扩容，可以保证常数的时间复杂度，而增加指定大小的容量只能达到O(n)的时间复杂度，因此，使用成倍的方式扩容。

---

## 11、Vector如何释放空间?

由于**vector的内存占用空间只增不减**，比如你首先分配了10,000个字节，然后erase掉后面9,999个，留下一个有效元素，但是内存占用仍为10,000个。所有内存空间是在vector析构时候才能被系统回收。empty()用来检测容器是否为空的，clear()可以清空所有元素。但是即使clear()，vector所占用的内存空间依然如故，无法保证内存的回收。

**如果需要空间动态缩小，可以考虑使用deque**

如果使用vector，可以用 `swap()` 来帮助你释放多余内存或者清空全部内存。

`vector<int>(vec).swap(vec);` 相当于 `vec.shrink_to_fit();`, 能清空vec中多余的空间

```c++
#include <iostream>
#include <vector>
using namespace std;

int main () {
    vector<int> vec (100,100);
    vec.push_back(1);
    vec.push_back(2);
    cout <<"vec.size(): " << vec.size() << endl;        // vec.size(): 102
    cout <<"vec.capasity(): " << vec.capacity() << endl; // vec.capasity(): 200

    vector<int>(vec).swap(vec); //清空vec中多余的空间，相当于vec.shrink_to_fit();
    cout <<"vec.size(): " << vec.size() << endl;    // vec.size(): 102
    cout <<"vec.capasity(): " << vec.capacity() << endl; // vec.capasity(): 102

    vector<int>().swap(vec); //清空vec的全部空间
    cout <<"vec.size(): " << vec.size() << endl; // vec.size(): 0
    cout <<"vec.capasity(): " << vec.capacity() << endl; // vec.capasity(): 0
    return 0;
}
```

---

## 12、容器内部删除一个元素

1.  顺序容器（序列式容器，比如vector、deque）

erase 迭代器不仅使所指向被删除的迭代器失效，而且使被删元素之后的所有迭代器失效(list除外)，所以**不能使用 erase(it++) 的方式，但是erase的返回值是下一个有效迭代器**

> It = c.erase(it);

2.  关联容器(关联式容器，比如map、set、multimap、multiset等)

erase 迭代器只是被删除元素的迭代器失效，但是返回值是 void，所以要采用 erase(it++) 的方式删除迭代器；

> c.erase(it++)

---

## 13、STL迭代器如何实现

1、 迭代器是一种抽象的设计理念，**通过迭代器可以在不了解容器内部原理的情况下遍历容器**，除此之外，STL中迭代器一个最重要的作用就是**作为容器与STL算法的粘合剂**

2、 **迭代器的作用就是提供一个遍历容器内部所有元素的接口，因此迭代器内部必须保存一个与容器相关联的指针，然后重载各种运算操作来遍历**，其中最重要的是 `*` 运算符与 `->` 运算符，以及 `++`、`--` 等可能需要重载的运算符重载。这和C++中的智能指针很像，智能指针也是将一个指针封装，然后通过引用计数或是其他方法完成自动释放内存的功能。

3、最常用的迭代器的相应型别有五种：value type、difference type、pointer、reference、iterator catagoly;

---

## 14、map、set是怎么实现的，红黑树是怎么能够同时实现这两种容器？ 为什么使用红黑树？

1.  他们的底层都是以红黑树的结构实现，因此插入删除等操作都在 O(logn) 时间内完成，因此可以完成高效的插入删除；
    
2.  在这里我们定义了一个模版参数，如果它是key那么它就是set，如果它是map，那么它就是map；底层是红黑树，实现map的红黑树的节点数据类型是key+value，而实现set的节点数据类型是value
    
3.  因为map和set要求是自动排序的，红黑树能够实现这一功能，而且时间复杂度比较低。
    
---

## 15、如何在共享内存上使用STL标准库？

在共享内存上使用STL标准库需要注意一些问题，因为STL容器通常在设计时假设在普通内存上使用，而共享内存有一些特殊的限制和注意事项。下面是一些在共享内存中使用STL标准库的常见方法：

1. 使用自定义的分配器：STL容器可以接受自定义的分配器来管理内存的分配和释放。你可以实现一个适合共享内存的自定义分配器，并将其传递给STL容器作为模板参数。这样容器将使用共享内存上的分配器进行内存管理。

2. 避免使用指针和引用：在共享内存上，指针和引用可能不再有效，因为共享内存在不同的进程间是有独立地址空间的。因此，尽量避免在共享内存中存储指针或引用类型的数据。

3. 使用无锁容器：在共享内存中使用锁可能导致死锁或同步问题。可以考虑使用无锁容器（lock-free containers）来避免多线程之间的同步问题。

4. 初始化共享内存：在共享内存上创建STL容器之前，确保正确地初始化共享内存区域，以及其中的STL容器。共享内存通常需要显式地进行初始化，确保所有使用的进程都能正确地访问和使用共享的STL容器。

5. 注意不同进程间的修改：在共享内存中，不同进程可能同时访问和修改STL容器。要注意并发访问导致的竞争条件和数据不一致性问题，可以通过合理的同步机制来避免这些问题。

6. 在不需要时销毁容器：确保在不再需要共享内存中的STL容器时及时销毁它们，以释放资源并避免资源泄漏。

---

## 16、map插入方式有哪几种？

- 用insert函数插入 `pair``数据，

> mapStudent.insert(pair<int, string>(1, "student_one")); 
    
- 用insert函数插入 `value_type` 数据

> mapStudent.insert(map<int, string>::value_type(1, "student_one"));
 
- 在insert函数中使用 `make_pair()` 函数

> mapStudent.insert(make_pair(1, "student_one")); 
    
- 用数组方式插入数据

> mapStudent[1] = "student_one"; 
    
---

## 17、STL中unordered_map(hash_map)和map的区别，hash_map如何解决冲突以及扩容

1.  `unordered_map` 和 `map` 类似，都是存储的 key-value 的值，可以通过key快速索引到value。不同的是 unordered_map 不会根据key的大小进行排序
    
2.  `unordered_map` 存储时是根据 key 的 hash 值判断元素是否相同，即内部元素是无序的; `map` 中的元素是按照二叉搜索树存储，进行中序遍历会得到有序遍历。
    
3.  所以使用时 `map` 的 `key` 需要定义 `operator <`。而 `unordered_map` 需要定义 `hash_value` 函数并且重载 `operator==`。但是很多系统内置的数据类型都自带这些，
    
4.  那么如果是自定义类型，那么就需要自己重载 `operator <` 或者 `hash_value()` 了。
    
5.  如果需要内部元素自动排序，使用 `map`，不需要排序使用 `unordered_map`
    
6.  `unordered_map` 的实现底层通常是基于哈希表（`hash_table`）的数据结构。 `hash_table` 使用的开链法来解决冲突
    
7.  当向 `unordered_map` 添加元素时，会根据当前容器的元素个数和负载因子（load factor）来判断是否需要进行扩容。负载因子是哈希表中存储元素数量与哈希桶数量之比。当元素个数大于等于当前数组长度乘以负载因子时，就会触发扩容操作
    
8.  扩容（resize）是重新计算容量，创建一个更大的哈希表，并将现有的元素重新哈希到新的哈希桶中。这样可以确保哈希表的负载因子在一个合理的范围内，以提高查找效率和降低冲突概率
    
---

## 18、vector越界访问下标，map越界访问下标？vector删除元素时会不会释放空间？

1.  通过下标访问vector中的元素时会做边界检查，但该处的实现方式要看具体IDE，不同IDE的实现方式不一样，确保不可访问越界地址。
    
2.  map的下标运算符 `[]` 的作用是：将key作为下标去执行查找，并返回相应的值；如果不存在这个key，就将一个具有该key和value的某人值插入这个map
    
3.  erase()函数，只能删除内容，不能改变容量大小;
    
erase()可以删除itVect迭代器指向的元素，并且返回要被删除的itVect之后的迭代器，迭代器相当于一个智能指针;
clear()函数，只能清空内容，不能改变容量大小;如果要想在删除内容的同时释放内存，那么你可以选择deque容器

---

## 19、map中 [] 与 find 的区别？

1.  map的下标运算符 `[]` 的作用是：将关键码作为下标去执行查找，并返回对应的值；如果不存在这个关键码，就将一个具有该关键码和值类型的默认值的项插入这个map
    
2.  map的find函数：用关键码执行查找，找到了返回该位置的迭代器；如果不存在这个关键码，就返回尾迭代器
   
---
    
## 20、 STL中 list 和 queue 与 vector 之间的区别

1.  list不再能够像vector一样以普通指针作为迭代器，因为其节点不保证在存储空间中连续存在；
    
2.  list插入操作原有的list迭代器失效;
    
3.  list不仅是一个双向链表，而且还是一个环状双向链表，所以它只需要一个指针；
    
4.  list不像vector那样有可能在空间不足时做重新配置、数据移动的操作，所以插入前的所有迭代器在插入操作之后都仍然有效；
    
5.  deque是一种双向开口的连续线性空间，所谓双向开口，意思是可以在头尾两端分别做元素的插入和删除操作；
    
6.  deque和vector最大的差异，一在于deque允许常数时间内对起头端进行元素的插入或移除操作；二在于deque没有所谓容量概念，因为它是动态地以分段连续空间组合而成，随时可以增加一段新的空间并链接起来，deque没有所谓的空间保留功能

---

## 21、STL中的allocator、deallocator

`allocator` 和 `deallocator` 是与STL（标准模板库）中内存管理相关的概念。STL中的 `allocator` 用于分配内存空间，而 `deallocator` 用于释放先前分配的内存空间。第一级配置器和第二级配置器是 SGI STL中的实现细节，旨在优化小额区块内存分配的性能

- 第一级配置器：
  
直接使用malloc()、free()和realloc()等标准C库函数来分配和释放大于128字节的内存区块。

- 第二级配置器：

主要针对小于等于128字节的内存区块，以降低内存碎片和额外负担。
主动将小额区块的内存需求量上调至8的倍数，以便更高效地管理内存池。
维护16个free-list，每个free-list管理不同大小（8~128字节）的小额区块

- 空间配置函数 `allocate()`：

首先判断区块大小，大于128字节时直接调用第一级配置器，使用malloc()等标准C库函数来分配内存。
小于等于128字节时，检查对应的free-list。
如果free-list中有可用区块，直接从中拿取并使用。
如果free-list中没有可用区块，将区块大小调整至8的倍数，然后调用refill()来重新为free-list分配内存。

- 空间释放函数 `deallocate()`

首先判断区块大小，大于128字节时直接调用第一级配置器，使用free()等标准C库函数来释放内存。
小于等于128字节时，找到对应的free-list，并将内存区块释放回free-list，以便后续重复利用。
这种内存池管理和free-list的实现可以减少频繁小内存的分配和释放带来的开销，同时更好地管理内存碎片。SGI STL采用了这样的内存管理策略，因为在其设计时考虑到C++标准库中很多数据结构和算法对小内存区块的需求较多，因此优化了内存分配的方式。不过需要注意，这些细节可能在不同的STL实现中会有所不同。
    
---

## 22、STL中hash table扩容发生什么？

1. 判断是否需要扩容：当哈希表中元素的数量达到某个阈值时（通常是当前容量的某个比例），需要进行扩容操作，以保持哈希表的高效性。

2. 创建新的更大的哈希表：在扩容时，STL会创建一个新的更大的哈希表，通常是当前容量的两倍或更多。

3. 将元素重新哈希：为了将元素重新分布到新的哈希表中，STL会将所有元素进行重新哈希计算，并根据新的哈希值将它们放置到新的桶（bucket）中。这涉及到重新计算元素的哈希值，然后将它们插入到新的桶中。

4. 将元素移动到新哈希表：STL中的hash table使用链表来解决哈希冲突，所以在元素重新哈希后，需要将它们从旧的桶中移动到新的桶中。这通常涉及到链表节点的移动操作。

5. 释放旧的哈希表：在扩容完成后，STL会释放旧的哈希表的内存空间。

通过以上步骤，STL的hash table成功完成了扩容操作，从而保持了哈希表的高效性能。扩容操作是非常重要的，因为它可以减少哈希冲突，保持哈希表的装载因子在一个合理的范围内，从而提高哈希表的查找、插入和删除操作的效率。
    
---

## 23、常见容器性质总结？

1. `vector` 底层数据结构为数组 ，支持快速随机访问

2. `list` 底层数据结构为双向链表，支持快速增删

3. `deque` 是一个双端队列, 也是在堆中保存内容的, 底层数据结构为一个中央控制器和多个缓冲区，详细见STL源码剖析P146，支持首尾（中间不能）快速增删，**也支持随机访问**

- 中央控制器：

`deque` 的中央控制器负责管理所有缓冲区，它知道 `deque` 的大小和当前的缓冲区布局。它维护了指向第一个缓冲区和最后一个缓冲区的指针，并跟踪所有缓冲区的数量。
中央控制器还负责在 `deque` 的两端插入和删除元素时进行调整。在某些情况下，可能需要重新分配新的缓冲区或回收空闲的缓冲区

- 缓冲区：

每个缓冲区都是一块连续的内存区域，用于存储元素。它们通常是固定大小的，比如一些实现中，每个缓冲区的大小为512个元素（这个大小可能会根据不同的STL实现而有所变化）。
缓冲区的数量和大小是可以动态增长的，当需要在 `deque` 的两端插入新元素时，可以添加新的缓冲区。
`deque` 的这种底层数据结构使得它在两端进行插入和删除操作的时间复杂度为常数时间（O(1)），这是因为在两端进行操作时，不需要移动所有元素，只需要调整中央控制器和缓冲区之间的指针即可。同时，由于每个缓冲区是一块连续的内存区域，`deque` 也支持随机访问，可以通过索引直接访问任意位置的元素，时间复杂度为O(1)

保存形式如 `[堆1] --> [堆2] -->[堆3] --> ...`

每个堆保存好几个元素,然后堆和堆之间有指针指向,看起来像是 `list` 和 `vector` 的结合品.

4. `stack` 底层一般用 `list` 或 `deque` 实现，封闭头部即可，不用 `vector` 的原因应该是容量大小有限制，扩容耗时

5. `queue` 底层一般用 `list` 或 `deque` 实现（ `stack` 和 `queue` 其实是适配器,而不叫容器，因为是对容器的再封装）

6. `priority_queue` 的底层数据结构一般为 `vector` 为底层容器，堆heap为处理规则来管理底层容器实现

7. `set` 底层数据结构为红黑树，有序，不重复; `multiset` 底层数据结构为红黑树，有序，可重复

8. `map` 底层数据结构为红黑树，有序，不重复; `multimap` 底层数据结构为红黑树，有序，可重复

9. `unordered_set` 底层数据结构为hash表，无序，不重复; `unordered_multiset` 底层数据结构为hash表，无序，可重复

10. `unordered_map` 底层数据结构为hash表，无序，不重复; `unordered_multimap` 底层数据结构为hash表，无序，可重复

---

## 24、vector的增加删除都是怎么做的？为什么是1.5或者是2倍？

1.  新增元素：vector通过一个连续的数组存放元素，如果集合已满，在新增数据的时候，就要分配一块更大的内存，将原来的数据复制过来，释放之前的内存，再插入新增的元素；
    
2.  对vector的任何操作，一旦引起空间重新配置，指向原vector的所有迭代器就都失效了 ；
    
3.  初始时刻vector的capacity为0，塞入第一个元素后capacity增加为1；
    
4.  不同的编译器实现的扩容方式不一样，VS2015中以1.5倍扩容，GCC以2倍扩容。
    
对比可以发现采用采用成倍方式扩容，可以保证常数的时间复杂度，而增加指定大小的容量只能达到O(n)的时间复杂度，因此，使用成倍的方式扩容。

1.  考虑可能产生的堆空间浪费，成倍增长倍数不能太大，使用较为广泛的扩容方式有两种，以2二倍的方式扩容，或者以1.5倍的方式扩容。
    
2.  以2倍的方式扩容，导致下一次申请的内存必然大于之前分配内存的总和，导致之前分配的内存不能再被使用，所以最好倍增长因子设置为(1,2)之间：
    
3.  向量容器vector的成员函数pop_back()可以删除最后一个元素.
    
4.  而函数erase()可以删除由一个iterator指出的元素，也可以删除一个指定范围的元素。
    
5.  还可以采用通用算法remove()来删除vector容器中的元素.
    
6.  不同的是：**采用 remove 一般情况下不会改变容器的大小，而 pop_back() 与 erase() 等成员函数会改变容器的大小**

---

## 25、说一下STL每种容器对应的迭代器

|容器|迭代器|
|---|---|
|`vector, deque`|随机访问迭代器|
|`stack, queue, priority_queue`|无|
|`list, (multi)set/map`|双向迭代器|
|`unordered_(multi)set/map, forward_list`|前向迭代器|

- `vector, deque`：支持随机访问迭代器。这意味着可以通过迭代器在O(1)时间复杂度内直接访问容器中的任意元素。vector和deque是连续存储的容器，因此可以使用指针算术来实现高效的随机访问。

- `stack, queue, priority_queue`：不支持迭代器。这些容器是适配器（adapter），它们对底层容器进行封装，并不直接提供迭代器访问。

- `list, (multi)set/map`：支持双向迭代器。双向迭代器可以在容器中前进和后退，但不能像随机访问迭代器那样在O(1)时间内进行直接访问。

- `unordered_(multi)set/map, forward_list`：支持前向迭代器。前向迭代器可以在容器中前进，但不能后退，也不能像随机访问迭代器那样在O(1)时间内进行直接访问。

---

## 26、STL中迭代器失效的情况有哪些？

- vector

尾后插入：size < capacity时，首迭代器不失效尾迭代器失效，size == capacity时，所有迭代器均失效（需要重新分配空间）

中间插入：中间插入：size < capacity时，首迭代器不失效但插入元素之后所有迭代器失效，size == capacity时，所有迭代器均失效。

尾后删除：只有尾迭代失效。

中间删除：删除位置之后所有迭代失效。

- deque 和 vector 的情况类似, 因为deque也需要重新分配空间来管理多个缓冲区

- list 双向链表每一个节点内存不连续, 删除节点仅当前迭代器失效, erase 返回下一个有效迭代器;

- map/set 等关联容器底层是红黑树删除节点不会影响其他节点的迭代器, 使用递增方法获取下一个迭代器 mmp.erase(iter++);

- unordered_(hash) 迭代器意义不大, rehash之后, 迭代器应该也是全部失效.

---

## 27、STL中vector的实现

`vector` 是一种序列式容器，其数据安排以及操作方式与 `array` 非常类似，两者的唯一差别就是对于空间运用的灵活性，众所周知，`array` 占用的是静态空间，一旦配置了就不可以改变大小，如果遇到空间不足的情况还要自行创建更大的空间，并手动将数据拷贝到新的空间中，再把原来的空间释放。 `vector` 则使用灵活的动态空间配置，维护一块**连续的线性空间**，在空间不足时，可以自动扩展空间容纳新元素，做到按需供给。其在扩充空间的过程中仍然需要经历：**重新配置空间，移动数据，释放原空间**等操作。这里需要说明一下动态扩容的规则：以原大小的两倍配置另外一块较大的空间（或者旧长度+新增元素的个数）

> const size_type len  = old_size + max(old_size, n);

Vector 扩容倍数与平台有关，在Win + VS 下是 1.5倍，在 Linux + GCC 下是 2 倍

需要注意的是，频繁对 `vector` 调用 `push_back()` 对性能是有影响的，这是因为每插入一个元素，如果空间够用的话还能直接插入，若空间不够用，则需要重新配置空间，移动数据，释放原空间等操作，对程序性能会造成一定的影响

---

## 28、STL中slist的实现

list是双向链表，而slist（single linked list）是单向链表，它们的主要区别在于：前者的迭代器是双向的Bidirectional iterator，后者的迭代器属于单向的Forward iterator。虽然slist的很多功能不如list灵活，但是其所耗用的空间更小，操作更快。

根据STL的习惯，**插入操作会将新元素插入到指定位置之前**，而非之后，然而slist是不能回头的，只能往后走，因此在slist的其他位置插入或者移除元素是十分不明智的，但是在slist开头却是可取的，slist特别提供了`insert_after()` 和 `erase_after()` 供灵活应用。考虑到效率问题，slist只提供 `push_front()` 操作，元素插入到 slist 后，存储的次序和输入的次序是相反的

slist的单向迭代器如下图所示：

![](http://oss.interviewguide.cn/img/202205071953335.png)

slist默认采用alloc空间配置器配置节点的空间，其数据结构主要代码如下

```c++
template <class T, class Allco = alloc>
class slist {
    ...
private:
    ...
    static list_node* create_node(const value_type& x){}//配置空间、构造元素
    static void destroy_node(list_node* node){}//析构函数、释放空间
private:
    list_node_base head; //头部
public:
    iterator begin(){}
    iterator end(){}
    size_type size(){}
    bool empty(){}
    void swap(slist& L){}//交换两个slist，只需要换head即可
    reference front(){} //取头部元素
    void push_front(const value& x){}//头部插入元素
    void pop_front(){}//从头部取走元素
    ...
}
```

```c++
#include <forward_list>
#include <algorithm>
#include <iostream>
using namespace std;

int main()
{
    forward_list<int> fl;
    fl.push_front(1);
    fl.push_front(3);
    fl.push_front(2);
    fl.push_front(6);
    fl.push_front(5);
    forward_list<int>::iterator ite1 = fl.begin();
    forward_list<int>::iterator ite2 = fl.end();
    for(;ite1 != ite2; ++ite1)
        cout << *ite1 <<" "; // 5 6 2 3 1
    ite1 = find(fl.begin(), fl.end(), 2);
    if (ite1 != ite2)
        fl.insert_after(ite1, 99);
    for (auto it : fl)
        cout << it << " ";  //5 6 2 99 3 1
    ite1 = find(fl.begin(), fl.end(), 6);
    if (ite1 != ite2)
        fl.erase_after(ite1);
    for (auto it : fl)
        cout << it << " ";  //5 6 99 3 1
    return 0;
}
```

需要注意的是C++标准委员会没有采用slist的名称，`forward_list` 在C++ 11中出现，它与slist的区别是没有 `size()` 方法

---

## 29、STL中list的实现

相比于 vector 的连续线型空间，list 显得复杂许多，但是它的好处在于插入或删除都只作用于一个元素空间，因此list对空间的运用是十分精准的，对任何位置元素的插入和删除都是常数时间。list 不能保证节点在存储空间中连续存储，也拥有迭代器，迭代器的“++”、“--”操作对于的是指针的操作，list提供的迭代器类型是双向迭代器：Bidirectional iterators。

list节点的结构见如下源码：

```c++
template <class T>
struct __list_node {
    typedef void* void_pointer;
    void_pointer prev;
    void_pointer next;
    T data;
}
```

因为list是一个双向链表，每个节点都有指向前一个节点和后一个节点的指针，所以在插入和删除操作之后，并不会造成原迭代器失效。这也是list相比于vector的一个优势之一

list是一个环形链表，这是因为list在内部维护了一个头尾相连的空白节点，使得整个链表形成一个环形结构。这个环形结构可以方便地在头部和尾部进行元素的插入和删除，而不需要特别处理边界情况

list的空间管理默认采用 alloc 作为空间配置器，为了方便的以节点大小为配置单位，还定义一个 list_node_allocator 函数可一次性配置多个节点空间

由于list的双向特性，其支持在头部 front 和尾部 back 两个方向进行 push 和 pop 操作，当然还支持 erase，splice，sort，merge，reverse，sort 等操作，这里不再详细阐述

---

## 30、STL中的deque的实现

vector 是单向开口（尾部）的连续线性空间，**deque 则是一种双向开口的连续线性空间**，虽然vector也可以在头尾进行元素操作，但是其头部操作的效率十分低下（主要是涉及到整体的移动）

![](http://oss.interviewguide.cn/img/202205071953980.png)

**deque和vector的最大差异一个是deque运行在常数时间内对头端进行元素操作，二是deque没有容量的概念，它是动态地以分段连续空间组合而成，可以随时增加一段新的空间并链接起来**

deque虽然也提供随机访问的迭代器，但是其迭代器并不是普通的指针，其复杂程度比vector高很多，因此除非必要，否则一般使用vector而非deque。如果需要对deque排序，可以先将deque中的元素复制到vector中，利用sort对vector排序，再将结果复制回deque

deque由一段一段的定量连续空间组成，一旦需要增加新的空间，只要配置一段定量连续空间拼接在头部或尾部即可，因此 **deque 的最大任务是如何维护这个整体的连续性**

deque的数据结构如下：

```c++
class deque {
    ...
protected:
    typedef pointer* map_pointer; // 指向map指针的指针
    map_pointer map; // 指向map
    size_type map_size; // map的大小
public:
    ...
    iterator begin();
    iterator end();
    ...
}
```

![](http://oss.interviewguide.cn/img/202205220021322.png)

deque内部有一个指针指向map，map是一小块连续空间，其中的每个元素称为一个节点，node，每个node都是一个指针，指向另一段较大的连续空间，称为缓冲区，这里就是deque中实际存放数据的区域，默认大小512bytes。整体结构如上图所示。

deque的迭代器数据结构如下：

```c++
struct __deque_iterator {
    ...
    T* cur;     // 迭代器所指缓冲区当前的元素
    T* first;   // 迭代器所指缓冲区第一个元素
    T* last;    // 迭代器所指缓冲区最后一个元素
    map_pointer node;   // 指向map中的node
    ...
}
```

从deque的迭代器数据结构可以看出，为了保持与容器联结，迭代器主要包含上述4个元素

![](http://oss.interviewguide.cn/img/202205220021453.png)

deque迭代器的“++”、“--”操作是远比vector迭代器繁琐，其主要工作在于缓冲区边界，如何从当前缓冲区跳到另一个缓冲区，当然deque内部在插入元素时，如果map中node数量全部使用完，且node指向的缓冲区也没有多余的空间，这时会配置新的map（2倍于当前+2的数量）来容纳更多的node，也就是可以指向更多的缓冲区。在deque删除元素时，也提供了元素的析构和空闲缓冲区空间的释放等机制

---

## 31、STL中stack和queue的实现

**stack**

stack（栈）是一种先进后出（First In Last Out）的数据结构，只有一个入口和出口，那就是栈顶，除了获取栈顶元素外，没有其他方法可以获取到内部的其他元素，其结构图如下：

![](http://oss.interviewguide.cn/img/202205220021348.png)

stack这种单向开口的数据结构很容易由**双向开口的deque和list**形成，只需要根据stack的性质对应移除某些接口即可实现，stack的源码如下：

```c++
template <class T, class Sequence = deque<T> >
class stack {
    ...
protected:
    Sequence c;
public:
    bool empty(){return c.empty();}
    size_type size() const{return c.size();}
    reference top() const {return c.back();}
    const_reference top() const{return c.back();}
    void push(const value_type& x){c.push_back(x);}
    void pop(){c.pop_back();}
};
```

从 stack 的数据结构可以看出，其所有操作都是围绕Sequence完成，而Sequence默认是deque数据结构。**stack 这种 “修改某种接口，形成另一种风貌” 的行为，成为 adapter(适配器)。常将其归类为container adapter 而非 container**

stack除了默认使用deque作为其底层容器之外，也可以使用双向开口的list，只需要在初始化stack时，将list作为第二个参数即可。**由于stack只能操作顶端的元素，因此其内部元素无法被访问，也不提供迭代器**

**queue**

queue（队列）是一种先进先出（First In First Out）的数据结构，只有一个入口和一个出口，分别位于最底端和最顶端，出口元素外，没有其他方法可以获取到内部的其他元素，其结构图如下：

![](http://oss.interviewguide.cn/img/202205220021436.png)

类似的，queue这种“先进先出”的数据结构很容易由双向开口的 deque 和 list 形成，只需要根据queue的性质对应移除某些接口即可实现，queue的源码如下：

```c++
template <class T, class Sequence = deque<T> >
class queue {
    ...
protected:
    Sequence c;
public:
    bool empty(){return c.empty();}
    size_type size() const{return c.size();}
    reference front() const {return c.front();}
    const_reference front() const{return c.front();}
    void push(const value_type& x){c.push_back(x);}
    void pop(){c.pop_front();}
};
```

**从 queue 的数据结构可以看出，其所有操作都也都是是围绕Sequence完成，Sequence 默认也是 deque 数据结构。queue也是一类container adapter**

同样，queue也可以使用list作为底层容器，不具有遍历功能，没有迭代器

---

## 32、STL中的 heap 的实现

heap（堆）并不是 STL 的容器组件，是 `priority_queue`（优先队列）的底层实现机制，因为binary max heap（大根堆）总是最大值位于堆的根部，优先级最高

binary heap本质是一种complete binary tree（完全二叉树），整棵binary tree除了最底层的叶节点之外，都是填满的，但是叶节点从左到右不会出现空隙，如下图所示就是一颗完全二叉树

![](http://oss.interviewguide.cn/img/202205220021792.png)

完全二叉树内没有任何节点漏洞，是非常紧凑的，这样的一个好处是可以使用 array 来存储所有的节点，因为当其中某个节点位于 i 处，其左节点必定位于 2i 处，右节点位于 2i+1 处，父节点位于 i/2（向下取整）处。这种以 array 表示 tree 的方式称为隐式表述法

因此我们可以使用一个array和一组heap算法来实现max heap（每个节点的值大于等于其子节点的值）和min heap（每个节点的值小于等于其子节点的值）。由于 array 不能动态的改变空间大小，用vector代替array是一个不错的选择。

那 heap 算法有哪些？常见有的插入、弹出、排序和构造算法，下面一一进行描述。

**`push_heap`插入算法**

由于完全二叉树的性质，新插入的元素一定是位于树的最底层作为叶子节点，并填补由左至右的第一个空格。事实上，在刚执行插入操作时，新元素位于底层 vecto r的 end() 处，之后是一个称为percolate up（上溯）的过程，举个例子如下图：

![](http://oss.interviewguide.cn/img/202205220022842.png)

新元素50在插入堆中后，先放在vector的end()存着，之后执行上溯过程，调整其根结点的位置，以便满足max heap的性质，如果了解大根堆的话，这个原理跟大根堆的调整过程是一样的。

**`pop_heap`算法**

heap的pop操作实际弹出的是根节点吗，但在heap内部执行 `pop_heap` 时，只是将其移动到vector的最后位置，然后再为这个被挤走的元素找到一个合适的安放位置，使整颗树满足完全二叉树的条件。这个被挤掉的元素首先会与根结点的两个子节点比较，并与较大的子节点更换位置，如此一直往下，直到这个被挤掉的元素大于左右两个子节点，或者下放到叶节点为止，这个过程称为percolate down（下溯）。举个例子：

![](http://oss.interviewguide.cn/img/202205220022993.png)

根节点68被pop之后，移到了vector的最底部，将24挤出，24被迫从根节点开始与其子节点进行比较，直到找到合适的位置安身，需要注意的是pop之后元素并没有被移走，如果要将其移走，可以使用pop_back()。

**`sort_heap`算法**

一言以蔽之，因为 `pop_heap` 可以将当前heap中的最大值置于底层容器vector的末尾，heap范围减1，那么不断的执行 `pop_heap` 直到树为空，即可得到一个递增序列。

**`make_heap`算法**

将一段数据转化为heap，一个一个数据插入，调用上面说的两种percolate算法即可。

```c++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

int main() {
    vector<int> v = { 0,1,2,3,4,5,6 };
    make_heap(v.begin(), v.end()); //以vector为底层容器
    for (auto i : v)
        cout << i << " "; // 6 4 5 3 1 0 2
    cout << endl;
    v.push_back(7);
    push_heap(v.begin(), v.end());
    for (auto i : v)
        cout << i << " "; // 7 6 5 4 1 0 2 3
    cout << endl;
    pop_heap(v.begin(), v.end());
    cout << v.back() << endl; // 7 
    v.pop_back();
    for (auto i : v)
        cout << i << " "; // 6 4 5 3 1 0 2
    cout << endl;
    sort_heap(v.begin(), v.end());
    for (auto i : v)
        cout << i << " "; // 0 1 2 3 4 5 6
    return 0;
}
```
---

## 33、STL中的 priority_queue 的实现

`priority_queue`，优先队列，是一个拥有权值观念的queue，它跟queue一样是顶部入口，底部出口，在插入元素时，元素并非按照插入次序排列，它会自动根据权值（通常是元素的实值）排列，权值最高，排在最前面，如下图所示

![](http://oss.interviewguide.cn/img/202205220022333.png)

默认情况下，`priority_queue` 使用一个max-heap完成，**底层容器使用的是一般为 vector 为底层容器，堆 heap 为处理规则来管理底层容器实现** 。`priority_queue` 的这种实现机制导致其不被归为容器，而是一种容器配接器。关键的源码如下：

```c++
template <class T, class Squence = vector<T>, 
class Compare = less<typename Sequence::value_tyoe> >
class priority_queue{
	...
protected:
    Sequence c; // 底层容器
    Compare comp; // 元素大小比较标准
public:
    bool empty() const {return c.empty();}
    size_type size() const {return c.size();}
    const_reference top() const {return c.front()}
    void push(const value_type& x)
    {
        c.push_heap(x);
        push_heap(c.begin(), c.end(), comp);
    }
    void pop()
    {
        pop_heap(c.begin(), c.end(), comp);
        c.pop_back();
    }
};
```

`priority_queue` 的所有元素，进出都有一定的规则，只有 queue 顶端的元素（权值最高者），才有机会被外界取用，**它没有遍历功能，也不提供迭代器**

---

## 34、STL中set的实现？

STL中的容器可分为序列式容器（sequence）和关联式容器（associative），set属于关联式容器。

set的特性是，所有元素都会根据元素的值自动被排序（默认升序），set 元素的键值就是实值，实值就是键值，set不允许有两个相同的键值

**set 不允许迭代器修改元素的值，其迭代器是一种 constance iterators**

标准的STL set 以 RB-tree（红黑树）作为底层机制，几乎所有的set操作行为都是转调用RB-tree的操作行为，这里补充一下红黑树的特性：

*   每个节点不是红色就是黑色
*   根结点为黑色
*   如果节点为红色，其子节点必为黑
*   任一节点至（NULL）树尾端的任何路径，所含的黑节点数量必相同

**关联式容器尽量使用其自身提供的find()函数查找指定的元素，效率更高，因为STL提供的find()函数是一种顺序搜索算法**

---

## 35、STL中 map 的实现

map的特性是所有元素会根据键值进行自动排序。map中所有的元素都是pair，拥有键值(key)和实值(value)两个部分，并且不允许元素有相同的key

**一旦map的key确定了，那么是无法修改的，但是可以修改这个key对应的value，因此map的迭代器既不是 constant iterator，也不是 mutable iterator**

标准STL map的底层机制是RB-tree（红黑树），另一种以hash table为底层机制实现的称为hash_map。map的架构如下图所示

![](http://oss.interviewguide.cn/img/202205220022980.png)

map的在构造时缺省采用递增排序key，也使用alloc配置器配置空间大小，需要注意的是在插入元素时，调用的是红黑树中的 insert_unique() 方法，而非 insert_euqal()（multimap使用）

需要注意的是 subscript（下标）操作既可以作为左值运用（修改内容）也可以作为右值运用（获取实值）。例如：

```c++
maps["abc"] = 1; // 左值运用
int num = masp["abd"]; // 右值运用
```
无论如何，subscript操作符都会先根据键值找出实值，源码如下：
```c++
T& operator[](const key_type& k) {	
    return (*((insert(value_type(k, T()))).first)).second;
}
```

代码运行过程是：首先根据键值和实值做出一个元素，这个元素的实值未知，因此产生一个与实值型别相同的临时对象替代：`value_type(k, T());`

再将这个对象插入到map中，并返回一个pair： `pair<iterator,bool> insert(value_type(k, T()));`

pair第一个元素是迭代器，指向当前插入的新元素，如果插入成功返回true，此时对应左值运用，根据键值插入实值。插入失败（重复插入）返回false，此时返回的是已经存在的元素，则可以取到它的实值

```c++
(insert(value_type(k, T()))).first;     // 迭代器
*((insert(value_type(k, T()))).first);  // 解引用
(*((insert(value_type(k, T()))).first)).second; // 取出实值
```

由于这个实值是以引用方式传递，因此作为左值或者右值都可以

---

## 36、set 和 map 的区别，multimap 和 multiset 的区别

set只提供一种数据类型的接口，但是会将这一个元素分配到key和value上，而且它的 `compare_function` 用的是 identity() 函数，这个函数是输入什么输出什么，这样就实现了set机制，**set 的 key 和 value 其实是一样的了。其实他保存的是两份元素，而不是只保存一份元素**

map则提供两种数据类型的接口，分别放在 key 和 value 的位置上，他的比较 function 采用的是红黑树的comparefunction()，保存的确实是两份元素。

他们两个的insert都是采用红黑树的 `insert_unique()` 独一无二的插入

multimap和map的唯一区别就是：multimap调用的是红黑树的 `insert_equal()`,可以重复插入而map调用的则是独一无二的插入 `insert_unique()`，multiset和set也一样，底层实现都是一样的，只是在插入的时候调用的方法不一样

**红黑树概念**

面试时候现场写红黑树代码的概率几乎为0，但是红黑树一些基本概念还是需要掌握的（红黑树是一种自平衡的二叉搜索树，即二叉排序树）。

1、它是二叉排序树（继承二叉排序树特点）：

*   若左子树不空，则左子树上所有结点的值均小于或等于它的根结点的值。
    
*   若右子树不空，则右子树上所有结点的值均大于或等于它的根结点的值。
    
    *   左、右子树也分别为二叉排序树。
    
2、它满足如下几点要求：
    
*   树中所有节点非红即黑
    
*   根节点必为黑节点
    
*   红节点的子节点必为黑（黑节点子节点可为黑），左右子树的高度差最大可以是两倍
    
*   从根到每个叶子节点 (NULL) 的任何路径上黑结点数相同, 这保证了红黑树的平衡性
    

3、这些特性确保了红黑树是自平衡的，避免了二叉搜索树在极端情况下退化成链表的问题，从而保证了查找时间一定可以控制在 O(logn)

---

## 37、STL中 unordered_map 和 map 的区别和应用场景

map支持键值的自动排序，底层机制是红黑树，红黑树的查询和维护时间复杂度均为 O(logn)，但是空间占用比较大，因为每个节点要保持父节点、孩子节点及颜色的信息

unordered_map是C++ 11新添加的容器，底层机制是哈希表，通过hash函数计算元素位置，其查询时间复杂度为O(1)，维护时间与bucket桶所维护的list长度有关，但是建立hash表耗时较大

从两者的底层机制和特点可以看出：map适用于有序数据的应用场景，unordered_map适用于高效查询的应用场景

---

## 38、hashtable中解决冲突有哪些方法？

**线性探测**

使用hash函数计算出的位置如果已经有元素占用了，则向后依次寻找，找到表尾则回到表头，直到找到一个空位

**开链**

每个表格维护一个list，如果hash函数计算出的格子相同，则按顺序存在这个list中

**再散列**

发生冲突时使用另一种hash函数再计算一个地址，直到不冲突

**二次探测**

使用hash函数计算出的位置如果已经有元素占用了，按照 1^2、2^2、3^2...的步长依次寻找，如果步长是随机数序列，则称之为伪随机探测

**公共溢出区**

一旦hash函数计算的结果相同，就放入公共溢出区