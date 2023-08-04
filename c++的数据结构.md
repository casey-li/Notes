
- [:lollipop: 堆](#lollipop-堆)
- [:lollipop: 智能指针](#lollipop-智能指针)

## :lollipop: 堆

最重要的操作：上浮以及下沉

下标从 0 开始, 那么第 i 个节点的左孩子为
```c++
// 大根堆
void up(vector<int>& nums, int i) {
    // 当前节点的值小于父节点的值，或者抵达父节点停止；i 为 0 时, (i - 1)/2也为0
    while (nums[i] > nums[(i - 1) / 2]) {    
        swap(nums[i], nums[(i - 1) / 2]);
        i = (i - 1)/2 ;
    }
}

// 用 heapSzie 控制堆的大小, 检查是否还有左右孩子
void down(vector<int>& nums, int i, int heapSize) {    
    int left = i * 2 + 1;
    while (left < heapSize) {
        int largest = left + 1 < heapSize && nums[left + 1] > nums[left] ? left + 1 : left;
        if (nums[i] >= nums[largest]) break;
        swap(nums[i], nums[largest]);
        i = largest;
        left = i * 2 + 1;
    }
}

void heapSort(vector<int> &nums) {
    // 构造大根堆
    int heapSize = nums.size();
    for (int i = 0; i < heapSize; ++i) {    // O(N)
        up(nums, i);                 // O(log N)
    }
    // 逐个交换元素, 得到从小到大的序列
    swap(nums[0], nums[--heapSize]);
    while (heapSize > 0) {           // O(N)
        down(nums, 0, heapSize);   // O(log N)
        swap(nums[0], nums[--heapSize]);    // O(1)
    }
}
```

完整的数据结构, 不考虑是否越界 (假设 `pop(), top()` 时必有元素)

```c++
#include <bits/stdc++.h>
using namespace std;
    
class Heap {
public :
    Heap(): heapSize(0) {}
    Heap(vector<int> nums): nums(nums), heapSize(nums.size()) {
        // 建堆
        for (int i = 0; i < heapSize; ++i) {    // O(N)
            up(i);                 // O(log N)
        }   
    }

    int top() {
        return nums[0];
    }

    void pop() {
        swap(nums[0], nums[--heapSize]);
        down(0);
    }

    void push(int val) {
        if (nums.size() > heapSize) {
            nums[heapSize++] = val;
            up(heapSize - 1);
        } else {
            nums.emplace_back(val);
            up(heapSize++);
        }
    }

    vector<int> heapSort() {
        int n = heapSize;
        while (heapSize) {
            pop();
        }
        return {nums.begin(), nums.begin() + n};
    }

    void print() {
        for (int i = 0; i < heapSize; ++i) {
            cout << nums[i] << ", ";
        }
        cout << endl;
    }

private:
    void up(int i) {
        // 当前节点的值小于父节点的值，或者抵达父节点停止；i为 0 时, (i-1)/2也为0
        while (nums[i] > nums[(i - 1) / 2]) {    
            swap(nums[i], nums[(i - 1) / 2]);
            i = (i - 1)/2 ;
        }
    }

    void down(int i) {    
        int left = i * 2 + 1;
        while (left < heapSize) {
            int largest = left + 1 < heapSize && nums[left + 1] > nums[left] ? left + 1 : left;
            if (nums[i] >= nums[largest]) break;
            swap(nums[i], nums[largest]);
            i = largest;
            left = i * 2 + 1;
        }
    }
    vector<int> nums;
    int heapSize;
};


int main() {
    Heap heap;
    for (int i = 0; i < 20; ++i) {
        heap.push(rand() % 100);
    }
    auto sorted = heap.heapSort();
    for (int &n : sorted) {
        cout << n << ", ";
    }
    return 0;
}
```

## :lollipop: 智能指针

资源管理 RAII 技术，利用对象的生命周期管理程序资源

- 构造函数

包括引用计数和模板指针, 注意需要加 explict, 否则会出问题

```c++
void func(MyShared_Ptr<int> ptr) {
    // ...
}

int main() {
    int* pInt = new int(42);
    func(pInt);  // 隐式转换为 MyShared_Ptr<int>
    cout << *pInt << endl;   // 随机值, 已经被回收了
    // ...
}
```

- 析构函数

减少引用计数的值, 只有当引用计数的值变为 0 了以后, 才应该真正释放内存, 否则会出现内存重复释放的问题

- 拷贝构造函数

增加引用计数 (需要判断一下传入对象的指针是否为空指针, 若为空指针的话跳过, 否则自增引用计数时访问了非法内存)

- 拷贝赋值运算符

减少左侧对象的引用计数 (**注意非空**, 若为 0 的话还应执行析构原空间), 增加右侧对象的引用计数

此外还应处理自赋值的情况, 所以应在检查引用计数是否变为 0 之前递增右对象的引用计数 (这样即使左右对象相等, 左侧对象递减引用计数后也不会析构指向的空间)

**传入的是 const 对象, 对象中的数据为两个指针, 所以只要不改指针的指向即可, 可以修改指针指向的值**

- 交换操作

非必须, 但是对于分配了资源的类，定义 `swap` 是一种很重要的优化手段

> 若直接 std::swap(a, b) 的话只会调用 std 的 swap()
> 为了调用自己的类的 swap(), 需要先 using std::swap; 
> 这样自己后面调用 swap() 的时候如果自己的类实现了 swap() 就调用自己的 swap(), 否则就调用 std::swap()

定义了 swap 的类可以使用 swap 来定义它们的赋值运算符, 使用**拷贝并交换**的技术 **(自动处理了自赋值并且天然就是异常安全的)**

```c++
// 构造临时对象, 调用 swap 来交换临时对象和 *this
// 结束后临时对象被销毁, 自动调用析构函数
MyShared_Ptr& operator=(const MyShared_Ptr& rhs) {
    MyShared_Ptr(rhs).swap(*this);
    return *this;
}
```

- 移动语义

注意需要加修饰符 `noexcept`, `std::move` 用于将左值转化为右值 (`static_cast<T &&>(lvaule)`)。将左值转化为右值后, 左值就不能继续使用了!!

- 重载指针的相关运算符 

`*` 和 `->`, 一个返回引用, 一个返回指针即可

`operator bool()`, 应是 explict 的, 只能转换为 bool 类型 (可以用在 if 中或进行 逻辑判断)

```c++
MyShared_Ptr<int> p1(new int(1)), p2;
if (p1) {} // true

if (p1 && p2) {} // false

p1 + 1; // 错误, 但若非 explict, p1 会转化为 bool 再转化为 int, 结果为 2
```

**整体框架：**
```c++
template<typename T>
class MyShared_Ptr {
public:
    explicit MyShared_Ptr(T* ptr = nullptr) {}

    ~MyShared_Ptr() {}

    MyShared_Ptr(const MyShared_Ptr &other) {}
    
    MyShared_Ptr(MyShared_Ptr &&other) noexcept {}

    MyShared_Ptr& operator=(const MyShared_Ptr &other) {}
    
    MyShared_Ptr& operator=(MyShared_Ptr &&other) noexcept {}

    void swap(MyShared_Ptr &other) {}

    T& operator*() const {}

    T* operator->() const {}

    size_t use_count() const {}

    explicit operator bool() const {}

    bool unique() const {}

private:
    T* m_ptr;
    size_t* m_count;
};
```

**完整实现：**
```c++
template<typename T>
class MyShared_Ptr {
public:
    explicit MyShared_Ptr(T* ptr = nullptr) : m_ptr(ptr), m_count(nullptr) {
        if (m_ptr) {
            m_count = new size_t(1);
        }
    }

    ~MyShared_Ptr() {
        if (!m_ptr) {
            return;
        }
        if (--*m_count == 0) {
            delete m_ptr;
            delete m_count;
        }
    }

    MyShared_Ptr(const MyShared_Ptr &other) : m_ptr(other.m_ptr), m_count(other.m_count) {
        if (m_ptr) {
            ++*m_count;
        }
    }
    
    MyShared_Ptr(MyShared_Ptr &&other) noexcept : m_ptr(other.m_ptr), m_count(other.m_count) {
        other.m_ptr = nullptr;
        other.m_count = nullptr;
    }

    MyShared_Ptr& operator=(const MyShared_Ptr &other) {
        if (m_ptr == other.ptr) {
            return *this;
        }
        ++*other.m_count;
        if (m_ptr && --*m_count == 0) {
            delete m_ptr;
            delete m_count;
        }
        m_ptr = other.m_ptr;
        m_count = other.m_count;
        return *this;
    }

    MyShared_Ptr& operator=(MyShared_Ptr &&other) noexcept {
        if (m_ptr == other.m_ptr) {
            return *this;
        }
        m_ptr = other.m_ptr;
        m_count = other.m_count;
        other.m_ptr = nullptr;
        other.m_count = nullptr;
        return *this;
    }

    // MyShared_Ptr& operator=(const MyShared_Ptr& rhs) {
    //    MyShared_Ptr(rhs).swap(*this);
    //    return *this;
    // }
    
    // MyShared_Ptr& operator=(MyShared_Ptr &&other) noexcept {
    //     MyShared_Ptr(std::move(other)).swap(*this);
    //     return *this;
    // }

    void swap(MyShared_Ptr &other) {
        using std::swap;
        swap(m_ptr, other.m_ptr);
        swap(m_count, other.m_count);
    }

    T& operator*() const {
        return *m_ptr;
    }

    T* operator->() const {
        return m_ptr;
    }

    size_t use_count() const {
        return *m_count;
    }

    explicit operator bool() const {
        return m_ptr;
    }

    bool unique() const { 
        return *m_count == 1; 
    }

private:
    T* m_ptr;
    size_t* m_count;
};
```