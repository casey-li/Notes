
## 项目介绍

### 项目概述，为什么要做这个项目


项⽬是在学习计⽹的过程中逐步搭建的，这个项⽬综合性⽐较强，从中既能学习Linux环境下的⼀些系统调⽤，也能熟悉⽹络编程和⼀些⽹络框架，其中也根据⾃⼰的理解加⼊了⼀些性能调优的⼿段

### 项目中的难点

### 遇到的困难，如何解决的，自己的收获

### 针对项目做了那些优化，跟别人不同的地方

### 用到了哪些设计模式

## 项目细节

### IO多路复用

### 并发模型

#### Reactor

#### Proactor


### 线程池

### 数据库连接池

- 为什么？

数据库的连接是一个很耗时的操作，也容易对数据库造成安全隐患。所以，在程序初始化的时候，集中创建多个数据库连接，并把他们集中管理，供程序使用，可以保证较快的数据库读写速度，还更加安全可靠。 在不使用 MySQL 连接池的情况下访问数据库，那么每一次创建数据库连接都需要经过如下步骤：

1. TCP 建立连接的三次握手（客户端与 MySQL 服务器的连接基于 TCP 协议）
2. MySQL 认证的三次握手
3. 真正的 SQL 执行
4. MySQL 的关闭
5. TCP 的四次握手关闭

![](https://camo.githubusercontent.com/50aaf88f8c4d588a4b3f0280aa17725bdbdf7ae0e2799380a1eceae044a7f2ca/68747470733a2f2f63646e2e6e6c61726b2e636f6d2f79757175652f302f323032322f706e672f32363735323037382f313636353332373038323330342d39623366626333652d623064392d346561632d626362632d3431386233383164646561382e706e6723617665726167654875653d25323366386634656626636c69656e7449643d7535393164633930612d656431322d342663726f703d302663726f703d302663726f703d312663726f703d312666726f6d3d7061737465266865696768743d3337362669643d753739303238363332266d617267696e3d2535426f626a6563742532304f626a656374253544266e616d653d31363635333237303739253238312532392e706e67266f726967696e4865696768743d363833266f726967696e57696474683d373138266f726967696e616c547970653d62696e61727926726174696f3d3126726f746174696f6e3d302673686f775469746c653d66616c73652673697a653d313236393338267374617475733d646f6e65267374796c653d6e6f6e65267461736b49643d7565636532373732662d303035642d343361342d396561622d3738643435303465343063267469746c653d2677696474683d3339352e34303030323434313430363235)



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

- 细节
  
设置了数据库连接池的连接数目的最小值以及最大值，额外新开一个线程检测是否需要增加或者删除连接数目

- 性能测试

(单线程 / 多线程) (使用 / 不使用) 数据库连接池插入多条数据，计算总耗时

### 并发性问题

### 日志模块

- 同步/异步
  
- 如果是同步日志，那么每次产生日志信息时，就需要将这条日志信息完全写入磁盘后才会执行后续程序。而磁盘 IO 是比较耗时的操作，如果有大批量的日志信息需要写入就会阻塞网络库的工作。
- 如果是异步日志，那么写日志消息只需要将日志的信息先进行存储，当累计到一定量或者经过一定时间时再将这些日志信息批量写入磁盘。而这个写入过程靠后台线程去执行，不会影响处理事件的其他线程。

经过对比可以得到，异步日志的方式对性能更加友好，而且可以减少磁盘 IO 函数的操作，一次写入更多的数据，提高效率

### 定时器模块

### HTTP解析模块

### 内存池（有时间的话再加）