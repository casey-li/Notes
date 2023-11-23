
- [:lollipop: 堆](#lollipop-堆)
- [:lollipop: 智能指针](#lollipop-智能指针)
- [:lollipop: Vector](#lollipop-vector)
- [:lollipop: 线程池](#lollipop-线程池)
- [:lollipop: 生产者消费者](#lollipop-生产者消费者)
- [:lollipop: 数据库基本操作](#lollipop-数据库基本操作)
- [:lollipop: Linux 基本操作](#lollipop-linux-基本操作)
- [:lollipop: Git 常见命令](#lollipop-git-常见命令)
- [:lollipop: 多个线程交替打印数字](#lollipop-多个线程交替打印数字)
- [:lollipop: 跳表](#lollipop-跳表)

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


智能指针的线程安全由两方面保证（引用计数本身的线程安全和智能指针指向对象的线程安全）


## :lollipop: Vector

[vector的模拟实现](https://mp.weixin.qq.com/s/IWvgnIWcbclXx0OHqei_Sg)

```c++
template<typename T>
class myVector {
public:
    myVector() : start(nullptr), finish(nullptr), endpos(nullptr) {}

    myVector(size_t n, const T& val = T()) : start(nullptr), finish(nullptr), endpos(nullptr) {
        reverse(n);
        for (size_t i = 0; i < n; ++i) {
            start[i] = val;
        }
        finish = start + n;
    }

    myVector(const myVector<T>& vec) {
        start = new T[vec.size()];
        for (size_t i = 0; i < vec.size(); ++i) {
            start[i] = vec[i];
        }
        finish = start + vec.size();
        end = finish;
    }

    ~myVector() {
        delete[] start;
        start = finish = endpos = nullptr;
    }

    void reverse(size_t n) {
        if (n > capacity()) {
            size_t sz = size();
            T *tmp = new T[n];
            if (start) {
                for (size_t i = 0; i < sz; ++i) {
                    tmp[i] = start[i];
                }
                delete[] start;
            }
            start = tmp;
            finish = start + sz;
            endpos = start + n;
        }
    }

    void resize(size_t n, const T &val = T()) {
        if (n < size()) {
            finish = start + n;
        } else if (n > size()) {
            T *tmp = new T[n];
            for (size_t i = 0; i < n; ++i) {
                if (i < size()) {
                    tmp[i] = start[i];
                } else {
                    tmp[i] = val;
                }
            }
            delete []start;
            start = tmp;
            finish = start + n;
            end = finish;
        }
    }

    void push_back(const T &val) {
        if (size() == capacity()) {
            size_t len = size() == 0 ? 2 : size() * 2;
            reverse(len);
        }
        *finish = val;
        ++finish;
    }

    void pop_back() {
        assert(size());
        --finish;
    }

    T *erase(T* pos) {
        assert(pos < finish && pos >= start);
        T *cur = pos;
        while (cur != finish) {
            *cur = *(cur + 1);
            cur++;
        }
        finish--;
        return pos;
    }

    T *insert(T *pos, const T &val) {
        assert(pos <= finish && pos >= start);
        size_t p = pos - start;
        if (finish == end) {
            reverse(capacity() + 1);
        }
        for (size_t i = size() - 1; i > p; --i) {
            start[i] = start[i - 1];
        }
        start[p] = val;
        return start + p;
    }

    myVector<T>& operator=(const myVector<T> &v) {
        if (start != v.start) {
            T *tmp = new T[v.size()];
            for (size_t i = 0; i < v.size(); ++i) {
                tmp[i] = v.start[i];
            }
            delete []start;
            start = tmp;
            finish = start + v.size();
            end = finish;
        }
        return *this;
    }

    size_t size() const {
        return finish - start;
    }

    size_t capacity() const {
        return endpos - start;
    }

    T* begin() {
        return start;
    }

    T* end() {
        return finish;
    }

    T& front() {
        return *start;
    }

    T& back() {
        return *(finish - 1);
    }

    T& operator[](size_t pos) {
        assert(pos < size());
        return *(start + pos);
    }

    const T& operator[](size_t pos) const {
        assert(pos < size());
        return *(start + pos);
    }

    bool empty() {
        return start == finish;
    }


private:
    T* start;
    T* finish; // 这里的 finish 指向的是最后一个数的下一个位置
    T* endpos;
};
```

## :lollipop: 线程池

```c++
#include <mutex>
#include <condition_variable>
#include <queue>
#include <thread>
#include <functional>
#include <cassert>
#include <iostream>

class ThreadPool {
public:
    ThreadPool() = default;

    explicit ThreadPool(size_t thread_num = 8);

    ~ThreadPool();

    void Work();

    template<typename T>
    void AddTask(T&& task);

private:
    // 结构体，池子
    struct Pool {
        std::mutex mtx;                           // 互斥锁
        std::condition_variable cond;             // 条件变量
        bool is_stop;                             // 线程池是否停止
        std::queue<std::function<void()>> tasks;  // 任务队列，存储无参且返回值为 void 的可调用对象 （待执行的任务）
    };
    
    std::shared_ptr<Pool> pool_; // 池子
};

template<typename T>
void ThreadPool::AddTask(T&& task) {
    {
        std::lock_guard<std::mutex> locker(pool_->mtx);
        // 使用 std::forward<T> 实现完美转发，确保参数的值类别保持不变。
        pool_->tasks.emplace(std::forward<T>(task));
    }
    pool_->cond.notify_one();
}


ThreadPool::ThreadPool(size_t thread_num) : pool_(std::make_shared<Pool>()) {
    assert(thread_num > 0);
    pool_->is_stop = false;
    // 创建线程并进行线程分离
    for (size_t i = 0; i < thread_num; ++i) {
        // c++ 的 std::thread 可以灵活的使用不同签名的工作函数
        // c 的 pthread.h 只接受 void *(*)(void *) 签名的函数 （要将工作函数设为全局函数或者静态成员函数）
        // 传递一个函数指针，并将 this 指针做为参数传递给它（成员函数有默认的this指针做为参数）
        std::thread(&ThreadPool::Work, this).detach();
    }
}

ThreadPool::~ThreadPool() {
    if (pool_) {
        {
            std::lock_guard<std::mutex> locker(pool_->mtx);
            pool_->is_stop = true;
        }
        pool_->cond.notify_all();
    }
}

void ThreadPool::Work() {
    std::unique_lock<std::mutex> locker(pool_->mtx); 
    while (true) {
        if (!pool_->tasks.empty()) {
            auto task = std::move(pool_->tasks.front());
            pool_->tasks.pop();
            locker.unlock(); // 解锁，让其他线程也可以去取任务工作
            task();          // 因为工作队列中保存的就是可调用对象，可以直接执行任务
            locker.lock();
        }
        else if (pool_->is_stop) break;
        else pool_->cond.wait(locker); // 当前无任务，等待唤醒
    }
}
```

## :lollipop: 生产者消费者

```c++
#include <iostream>
#include <thread>
#include <vector>
#include <queue>
#include <mutex>
#include <condition_variable>

const int BUFFER_SIZE = 5;

std::queue<int> buffer;
std::mutex mtx;
std::condition_variable buffer_empty, buffer_full;

void producer(int id) {
    for (int i = 0; i < 10; ++i) {
        std::unique_lock<std::mutex> lock(mtx);

        // 如果缓冲区已满，等待消费者消费数据
        while (buffer.size() >= BUFFER_SIZE) {
            buffer_full.wait(lock);
        }

        int item = rand() % 100;
        buffer.push(item);
        std::cout << "Producer " << id << " produced: " << item << std::endl;

        // 通知消费者可以消费数据了
        buffer_empty.notify_all();
    }
}

void consumer(int id) {
    for (int i = 0; i < 10; ++i) {
        std::unique_lock<std::mutex> lock(mtx);

        // 如果缓冲区为空，等待生产者生产数据
        while (buffer.empty()) {
            buffer_empty.wait(lock);
        }

        int item = buffer.front();
        buffer.pop();
        std::cout << "Consumer " << id << " consumed: " << item << std::endl;

        // 通知生产者可以继续生产数据了
        buffer_full.notify_all();
    }
}

int main() {
    std::vector<std::thread> producers, consumers;
    for (int i = 0; i < 3; ++i) {
        producers.emplace_back(producer, i);
        consumers.emplace_back(consumer, i);
    }
    for (int i = 0; i < 3; ++i) {
        producers[i].join();
        consumers[i].join();
    }
    return 0;
}
```

## :lollipop: 数据库基本操作

```sql
1. 增加（Insert）： 用于将新的数据行插入到表中。

INSERT INTO table_name (column1, column2, ...) 
VALUES (value1, value2, ...);

2. 删除（Delete）： 用于从表中删除数据行。

DELETE FROM table_name WHERE condition;

3. 查询（Select）： 用于从表中检索数据。

SELECT column1, column2, ... 
FROM table_name WHERE condition;

4. 更新（Update）： 用于修改表中的数据行。

UPDATE table_name 
SET column1 = value1, column2 = value2, ... 
WHERE condition;

关于改变表结构（如添加、修改或删除表的列），可以使用以下SQL关键字：

1. 创建表（Create Table）： 用于创建一个新的数据库表。

CREATE TABLE table_name 
(column1 datatype, 
column2 datatype, ...);

2. 修改表（Alter Table）： 用于修改现有的表结构，包括添加、修改和删除列等操作。

添加列：ALTER TABLE table_name ADD column_name datatype;
修改列：ALTER TABLE table_name MODIFY column_name new_datatype;
删除列：ALTER TABLE table_name DROP COLUMN column_name;

3. 删除表（Drop Table）： 用于删除整个表及其数据。

DROP TABLE table_name;
```

## :lollipop: Linux 基本操作

```shell
1. **常用的Linux命令：**
   - `cd`: 切换目录。
   - `pwd`: 显示当前工作目录。
   - `cat`: 查看文件内容。
   - `grep`: 在文件中搜索指定的字符串。
   - `find`: 查找文件或目录。
   - `mv`: 移动或重命名文件。
   - `cp`: 复制文件或目录。
   - `rm`: 删除文件或目录。
   - `mkdir`: 创建目录。
   - `touch`: 创建空文件或更新文件的时间戳。
   - `vi` 或 `nano`: 文本编辑器。

2. **显示文件的第二行和最后一行：**
   - 若要显示文件的第二行，可以使用`head`命令：
     head -n 2 文件名

   - 若要显示文件的最后一行，可以使用`tail`命令：
     tail -n 1 文件名

3. **查找以.c结尾的文件：**
   - 使用`find`命令来查找以.c结尾的文件，例如在当前目录及其子目录中查找：
     find . -type f -name "*.c"
   - 这将列出所有以.c结尾的文件及其路径。

# 把文件中的 "apple" 替换成 "orange"
awk '{gsub("apple", "orange")} 1' filename.txt > newfile.txt

- gsub("apple", "orange")：这是 awk 的内置函数 gsub，用于全局替换字符串。它将 "apple" 替换为 "orange"
- {}：用来包裹 awk 的操作。
- 1：这是一个条件，它会打印每一行。在这种情况下，它打印了经过替换后的每一行。
```

## :lollipop: Git 常见命令

```shell

1. **常用的Git命令：**
   - `git init`: 在目录中初始化一个新的Git仓库。
   - `git clone`: 克隆现有的Git仓库。
   - `git add`: 将文件或更改添加到暂存区。
   - `git commit`: 提交暂存区的更改到本地仓库。
   - `git status`: 查看工作目录和暂存区的状态。
   - `git log`: 查看提交日志。
   - `git diff`: 查看工作目录中的更改。
   - `git branch`: 查看分支信息。
   - `git checkout`: 切换分支或恢复文件。
   - `git merge`: 合并分支。
   - `git pull`: 从远程仓库拉取更改。
   - `git push`: 推送本地更改到远程仓库。
   - `git remote`: 管理远程仓库。

2. **回滚操作：**
   - 如果你需要回滚到之前的提交状态，可以使用`git reset`命令。有三种主要的重置模式：
     - `git reset --soft`: 保留本地更改，并将HEAD指针移动到指定的提交，暂存区保持不变，可以重新提交。
     - `git reset --mixed`（默认模式）: 保留本地更改，但取消暂存区的更改，可以重新暂存和提交。
     - `git reset --hard`: 本地工作目录和暂存区都将回滚到指定的提交，慎用，可能会导致未保存的更改丢失。

   例如，如果要回滚到前一个提交：
   git reset HEAD~1

   如果要回滚到特定提交的SHA-1哈希值：
   git reset <commit-SHA>
```


## :lollipop: 多个线程交替打印数字

```c++
#include <iostream>
#include <thread>
#include <mutex>
#include <condition_variable>

std::mutex mtx;
std::condition_variable cv;
bool print_123 = true;

void printNumbers(const int num) {
    while (true) {
        std::unique_lock<std::mutex> lock(mtx);
        // 等待条件满足
        cv.wait(lock, [num] { return (num == 123 && print_123) || (num == 456 && !print_123); });
        // 打印数字
        std::cout << num << std::endl;
        // 更新条件变量状态
        print_123 = !print_123;
        // 通知另一个线程继续执行
        cv.notify_one();
    }
}

// 交替打印 123, 456
int main() {
    std::thread t1(printNumbers, 123);
    std::thread t2(printNumbers, 456);
    t1.join();
    t2.join();
    return 0;
}
```

## :lollipop: 跳表

```c++
#include <iostream>
#include <ctime>
#include <cstdlib>

// 定义跳表节点
struct SkipListNode {
    int value;
    SkipListNode** forward; // 数组，包含向前的指针
    int level; // 节点的级别

    SkipListNode(int val, int l);
};

SkipListNode::SkipListNode(int val, int l) {
    value = val;
    level = l;
    forward = new SkipListNode*[level + 1];
    for (int i = 0; i <= level; i++) {
        forward[i] = nullptr;
    }
}

// 定义跳表
class SkipList {
public:
    SkipList(int maxLevels);
    ~SkipList();
    bool search(int target);
    void insert(int value);
    void remove(int value);
    void display();

private:
    int maxLevels;
    int currentLevel;
    SkipListNode* head;
    double probability = 0.5; // 调整跳表层数的概率
    int randomLevel();
};

SkipList::SkipList(int maxLevels) {
    this->maxLevels = maxLevels;
    currentLevel = 0;
    head = new SkipListNode(-1, maxLevels);
}

SkipList::~SkipList() {
    delete head;
}

// 随机生成节点的级别
int SkipList::randomLevel() {
    int level = 0;
    while ((rand() / (double)RAND_MAX) < probability && level < maxLevels) {
        level++;
    }
    return level;
}

// 查找节点
bool SkipList::search(int target) {
    SkipListNode* current = head;
    for (int i = currentLevel; i >= 0; i--) {
        while (current->forward[i] && current->forward[i]->value < target) {
            current = current->forward[i];
        }
    }
    current = current->forward[0];
    return (current && current->value == target);
}

// 插入节点
void SkipList::insert(int value) {
    int newLevel = randomLevel();
    if (newLevel > currentLevel) {
        for (int i = currentLevel + 1; i <= newLevel; i++) {
            head->forward[i] = head;
        }
        currentLevel = newLevel;
    }

    SkipListNode* newNode = new SkipListNode(value, newLevel);
    SkipListNode* current = head;
    for (int i = currentLevel; i >= 0; i--) {
        while (current->forward[i] && current->forward[i]->value < value) {
            current = current->forward[i];
        }
        if (i <= newLevel) {
            newNode->forward[i] = current->forward[i];
            current->forward[i] = newNode;
        }
    }
}

// 删除节点
void SkipList::remove(int value) {
    SkipListNode* current = head;
    for (int i = currentLevel; i >= 0; i--) {
        while (current->forward[i] && current->forward[i]->value < value) {
            current = current->forward[i];
        }
    }
    current = current->forward[0];
    if (current && current->value == value) {
        for (int i = 0; i <= currentLevel; i++) {
            if (current->forward[i] && current->forward[i]->value != value) {
                break;
            }
            current->forward[i] = current->forward[i]->forward[i];
        }
        delete current;
    }
}

// 显示跳表内容
void SkipList::display() {
    for (int i = 0; i <= currentLevel; i++) {
        SkipListNode* current = head->forward[i];
        std::cout << "Level " << i << ": ";
        while (current) {
            std::cout << current->value << " ";
            current = current->forward[i];
        }
        std::cout << std::endl;
    }
}

int main() {
    srand(static_cast<unsigned>(time(nullptr));
    SkipList skipList(4); // 最大级别为4

    skipList.insert(10);
    skipList.insert(20);
    skipList.insert(30);
    skipList.insert(40);
    skipList.insert(50);

    std::cout << "Skip List content:" << std::endl;
    skipList.display();

    std::cout << "Searching for 30: " << (skipList.search(30) ? "Found" : "Not Found") << std::endl;
    std::cout << "Searching for 25: " << (skipList.search(25) ? "Found" : "Not Found") << std::endl;

    skipList.remove(30);
    skipList.remove(20);

    std::cout << "Skip List content after removal:" << std::endl;
    skipList.display();

    return 0;
}

```