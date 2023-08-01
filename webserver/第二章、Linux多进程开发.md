
- [lesson01 进程概述](#lesson01-进程概述)
- [lesson02 进程状态转换](#lesson02-进程状态转换)
  - [一、进程的状态](#一进程的状态)
  - [二、进程相关命令](#二进程相关命令)
  - [三、进程号和相关函数](#三进程号和相关函数)
- [lesson03-05：进程创建](#lesson03-05进程创建)
  - [一、父子进程之间的关系](#一父子进程之间的关系)
  - [二、GDB 多进程调试](#二gdb-多进程调试)
- [lesson06 exec函数族](#lesson06-exec函数族)
- [lesson07-09 进程控制](#lesson07-09-进程控制)
  - [案例](#案例)
- [lesson10-29 进程间通信](#lesson10-29-进程间通信)
  - [一、匿名管道](#一匿名管道)
    - [1、管道的特点](#1管道的特点)
    - [2、匿名管道的使用](#2匿名管道的使用)
      - [案例：实现 ps aux, 父子进程间通信](#案例实现-ps-aux-父子进程间通信)
    - [3、总结：](#3总结)
  - [二、有名管道](#二有名管道)
    - [有名管道的使用](#有名管道的使用)
  - [三、内存映射](#三内存映射)
    - [1、内存映射相关系统调用](#1内存映射相关系统调用)
    - [2、使用内存映射实现进程间通信](#2使用内存映射实现进程间通信)
      - [案例：内存映射做文件拷贝 (可以这么用，但若文件很大内存可能放不下)](#案例内存映射做文件拷贝-可以这么用但若文件很大内存可能放不下)
      - [案例：匿名映射实现进程间通信](#案例匿名映射实现进程间通信)
  - [四、信号](#四信号)
    - [1、简介](#1简介)
    - [2、信号相关的函数](#2信号相关的函数)
    - [3、信号捕捉函数](#3信号捕捉函数)
    - [4、信号集](#4信号集)
      - [信号集相关的函数](#信号集相关的函数)
      - [SIGCHLD信号](#sigchld信号)
      - [案例：使用`SIGCHLD`信号解决僵尸进程的问题。](#案例使用sigchld信号解决僵尸进程的问题)
  - [五、共享内存](#五共享内存)
    - [1、共享内存使用步骤](#1共享内存使用步骤)
    - [2、共享内存操作函数](#2共享内存操作函数)
    - [3、共享内存操作命令](#3共享内存操作命令)
- [lesson30-31 守护进程](#lesson30-31-守护进程)
  - [一、概念](#一概念)
  - [二、进程组、会话操作函数](#二进程组会话操作函数)
  - [三、守护进程](#三守护进程)
    - [守护进程的创建步骤](#守护进程的创建步骤)
    - [案例：创建守护进程，每隔2s获取一下系统时间，将这个时间写入到磁盘文件中。](#案例创建守护进程每隔2s获取一下系统时间将这个时间写入到磁盘文件中)


# lesson01 进程概述
    
***程序和进程***

- 程序：包含一系列信息的文件，这些信息描述了如何在运行时创建一个进程
- 进程：正在运行的程序的实例。它是操作系统动态执行的基本单元，在传统的操作系统中，进程既是基本的分配单元，也是基本的执行单元

一个程序可以创建多个进程，进程是由内核定义的抽象实体，并为该实体分配用以执行程序的各项系统资源。从内核的角度看，**进程由用户内存空间和一系列内核数据结构组成**。

用户内存空间包含了程序代码及代码所使用的变量，内核数据结构则用于维护进程状态信息 (与进程相关的标识号（IDs）、虚拟内存表、打开文件的描述符表、信号传递及处理的有关信息、...)

***并行和并发***
- 并行(parallel)：指在同一时刻，有多条指令在多个处理器上同时执行
- 并发(concurrency)：指在同一时刻只能有一条指令执行，但多个进程指令被快速的轮换执行，使得在宏观上具有多个进程同时执行的效果

***进程控制块（PCB）***
内核为每个进程分配一个 PCB(Processing Control Block)进程控制块，维护进程相关的信息，Linux 内核的进程控制块是 task_struct 结构体，包括：进程id, 进程的状态, 进程切换时需要保存和恢复的一些CPU寄存器, 描述虚拟地址空间的信息, ...

# lesson02 进程状态转换

## 一、进程的状态

**三态模型**

![进程三态模型](https://github.com/casey-li/Notes/blob/main/webserver/pic/%E7%AC%AC%E4%BA%8C%E7%AB%A0%E3%80%81Linux%E5%A4%9A%E8%BF%9B%E7%A8%8B%E5%BC%80%E5%8F%91/%E8%BF%9B%E7%A8%8B%E4%B8%89%E6%80%81%E6%A8%A1%E5%9E%8B.png?raw=true)

**五态模型**

![进程五态模型](https://github.com/casey-li/Notes/blob/main/webserver/pic/%E7%AC%AC%E4%BA%8C%E7%AB%A0%E3%80%81Linux%E5%A4%9A%E8%BF%9B%E7%A8%8B%E5%BC%80%E5%8F%91/%E8%BF%9B%E7%A8%8B%E4%BA%94%E6%80%81%E6%A8%A1%E5%9E%8B.png?raw=true)

- 运行态：进程占有处理器正在运行
- 就绪态：当进程已分配到除CPU以外的所有必要资源。在一个系统中处于就绪状态的进程可能有多个，通常将它们排成一个队列，称为就绪队列
- 阻塞态：又称为等待(wait)态或睡眠(sleep)态，指进程不具备运行条件，正在等待某个事件的完成
- 新建态：进程刚被创建时的状态，尚未进入就绪队列
- 终止态：进程完成任务到达正常结束点，或出现无法克服的错误而异常终止，或被操作系统及有终止权的进程所终止时所处的状态。进入终止态的进程以后不再执行，但依然保留在操作系统中等待善后。一旦其他进程完成了对终止态进程的信息抽取之后，操作系统将删除该进程。

## 二、进程相关命令
```c++
查看进程: ps aux / ajx
    STAT参数意义：
        D 不可中断 Uninterruptible（usually IO）
        R 正在运行，或在队列中的进程
        S(大写) 处于休眠状态
        T 停止或被追踪
        Z 僵尸进程
        ...

实时显示进程动态：top
    可以在使用 top 命令时加上 -d 来指定显示信息更新的时间间隔，执行后，可以按以下按键对显示的结果进行排序：
        M 根据内存使用量排序
        P 根据 CPU 占有率排序
        T 根据进程运行时间长短排序
        U 根据用户名来筛选进程
        K 输入指定的 PID 杀死进程

杀死进程：
    kill [-signal] pid
    kill –l 列出所有信号
    kill –SIGKILL 进程ID
    kill -9 进程ID
    killall name 根据进程名杀死进程
```

## 三、进程号和相关函数
1. 每个进程都由进程号来标识，其类型为 pid_t（整型），进程号的范围：0～32767。进程号总是唯一的，但可以重用。当一个进程终止后，其进程号就可以再次使用
2. 任何进程（除 init 进程）都是由另一个进程创建，该进程称为被创建进程的父进程，对应的进程号称为父进程号（PPID）
3. 进程组是一个或多个进程的集合。他们之间相互关联，进程组可以接收同一终端的各种信号，关联的进程有一个进程组号（PGID）。默认情况下，当前的进程号会当做当前的进程组号
4. 进程号和进程组相关函数：
    - `pid_t getpid(void);`
    - `pid_t getppid(void);`
    - `pid_t getpgid(pid_t pid);`

# lesson03-05：进程创建

系统允许一个进程创建新进程，新进程即为子进程，子进程还可以创建新的子进程，形成进程树结构模型。
- 头文件: `sys/types.h, unistd.h`
- 函数: `pid_t fork(void);`
- 返回值: 父子进程各自有返回值

在父进程中返回创建的子进程的ID，在子进程中返回0
在父进程中返回-1，表示创建子进程失败 (子进程数目达到上限, 内存不足)，并且设置errno

## 一、父子进程之间的关系

***区别***
1. fork()函数的返回值不同
    - 父进程中: >0 返回的子进程的ID
    - 子进程中: =0
2. pcb中的一些数据
    - 当前的进程的id pid
    - 当前的进程的父进程的id ppid
    - 信号集

***共同点***

当子进程刚被创建出来，还没有执行任何的写数据的操作时，用户区的数据和文件描述符表相同

***父子进程对变量是不是共享的？***

**读时共享（子进程被创建，两个进程没有做任何的写的操作），写时拷贝**

实际上，Linux 的 fork() 使用的是写时拷贝(copy on write)实现，写时拷贝是一种可以推迟甚至避免拷贝数据的技术

内核此时并不复制整个进程的地址空间，而是让父子进程共享同一个地址空间，只有在需要写入的时候才会复制地址空间，从而使各个进程拥有各自的地址空间

注意：fork() 之后父子进程共享文件，fork产生的父子进程有相同的文件描述符指向相同的文件表，引用计数增加，共享文件偏移指针

## 二、GDB 多进程调试

GDB 默认只能跟踪一个进程，可以在 fork 函数调用之前，通过指令设置 GDB 调试工具跟踪父进程或者是跟踪子进程，默认跟踪父进程。

- 查看跟踪哪个进程 `show follow-fork-mode`
- 查看调试模式 `show detach-on-fork`
- 设置调试父进程或者子进程：`set follow-fork-mode [parent（默认）| child]`
- 设置调试模式：`set detach-on-fork [on | off]`，默认为 on，表示调试当前进程的时候，其它的进程继续运行，如果为 off，调试当前进程的时候，其它进程被 GDB 挂起。
- 查看调试的进程：`info inferiors`
- 切换当前调试的进程：`inferior id`
- 使进程脱离 GDB 调试：`detach inferiors id`

# lesson06 exec函数族

exec 函数族的作用是根据指定的文件名找到可执行文件，并用它来取代调用进程的内容(不同于fork() 创建一个子进程)，换句话说，就是在调用进程内部执行一个可执行文件。因此一般是先创建一个子进程，然后在子进程中执行exec 函数族以替换子进程中的内容

exec 函数族的函数执行成功后不会返回，因为调用进程的实体，包括代码段，数据段和堆栈等都已经被新的内容取代，只留下进程 ID 等一些表面上的信息仍保持原样。只有调用失败了，它们才会返回 -1，从原程序的调用点接着往下执行。

```c++
int execl(const char *path, const char *arg, ...); //unistd.h
    - 参数：
        - path: 需要指定的执行的文件的路径或者名称
            a.out 或 /home/nowcoder/a.out 推荐使用绝对路径
            例子: execl("/bin/ps", "ps", "aux", NULL); //执行shell命令，本质上就是一个可执行文件，注意最后要以NULL结束

        - arg: 是执行可执行文件所需要的参数列表
            第一个参数一般没有什么作用，为了方便，一般写的是执行的程序的名称
            从第二个参数开始往后，就是程序执行所需要的的参数列表。
            参数最后需要以NULL结束（哨兵）

    - 返回值：
        只有当调用失败，才会有返回值，返回-1，并且设置errno
        如果调用成功，没有返回值 (原先的用户区都已经被替换了，调用execl的代码都没了)

    通常创建一个子进程，在子进程中执行exec函数族中的函数，否则原来执行的代码会被替换

int execlp(const char *file, const char *arg, ... ); //unistd.h
    - 会到环境变量中查找指定的可执行文件，如果找到了就执行，找不到就执行不成功。
    - 参数：
        - file:需要执行的可执行文件的文件名
            a.out
            ps (不需要写/bin/ps, 若execl()直接写ps则不会执行ps，执行原代码，并设置erron)

        - arg: 同 execl()

    - 返回值：同 execl()

    查看环境变量: env
    若执行自己的可执行文件，如在同目录下的一个 a.out，会出错，因为不在环境变量的path中

int execv(const char *path, char *const argv[]);
    功能同execl()，只不过用数组传递参数
    - argv是需要的参数的一个字符串数组
    char * argv[] = {"ps", "aux", NULL};
    execv("/bin/ps", argv);

int execve(const char *filename, char *const argv[], char *const envp[]);
    功能同execlp()，只不过用数组自己指定环境变量
    char * envp[] = {"/home/nowcoder", "/home/bbb", "/home/aaa"};

int execle(const char *path, const char *arg, ...);
int execvp(const char *file, char *const argv[]);
int execvpe(const char *file, char *const argv[], char *const envp[]);

函数名中的字符:
    l(list) 参数地址列表，以空指针结尾
    v(vector) 存有各参数地址的指针数组的地址
    p(path) 按 PATH 环境变量指定的目录搜索可执行文件
    e(environment) 存有环境变量字符串地址的指针数组的地址

最常用的:
    int execl(const char *path, const char *arg, ...);
    int execlp(const char *file, const char *arg, ... );
exec函数族; 再次体现了读时共享，写时拷贝机制的优越性。若无该机制则在fork()时子进程拷贝了一份虚拟地址空间，执行exec又重新覆盖，之前的就白拷贝了，浪费时间，浪费内存
```

# lesson07-09 进程控制

***进程创建*** : `fork()`

***进程退出***

```c++
void exit(int status); //标准c库里的，stdlib.h
    会多做一些事情，如刷新I/O缓冲，关闭文件描述符，再调用 _exit();s
void _exit(int status); //Linux 系统调用，unistd.h
    status参数：是进程退出时的一个状态信息。父进程回收子进程资源的时候可以获取到。
```

***孤儿进程***

父进程先于子进程终止，子进程会变为孤儿进程（Orphan Process），之后内核就把孤儿进程的父进程设置为 `init` 进程，由它处理善后工作，因此孤儿进程并不会有什么危害。

***僵尸进程***

每个进程结束之后, 都会释放自己地址空间中的用户区数据，但内核区的 PCB 需要父进程去释放

在父进程尚未对终止的子进程的残留资源（PCB）进行回收的期间（如父进程处于死循环中），子进程就会存放于内核中变成僵尸（Zombie）进程。

僵尸进程不能被 `kill -9` 杀死，因为它们本来就处于终止态

危害：若父进程不调用 `wait()` 或 `waitpid()` 的话，那么保留的那段信息就不会释放，其进程号就会一直被占用。在有限的进程号资源下，若存在大量僵尸进程，系统可能不能产生新的进程

***进程回收***

在每个进程退出的时候，内核释放该进程所有的资源、包括打开的文件、占用的内存等。但是仍然为其保留一定的信息，这些信息主要指进程控制块PCB的信息（包括进程号、退出状态、运行时间等）。

父进程可以通过调用 `wait()` 或 `waitpid()` 得到它的退出状态同时彻底清除掉这个进程

wait() 和 waitpid() 函数的功能一样，区别在于，wait() 函数会阻塞，waitpid() 可以设置不阻塞，waitpid() 还可以指定等待哪个子进程结束

注意：一次wait或waitpid调用只能清理一个子进程，清理多个子进程应使用循环。

```c++
pid_t wait(int *wstatus); // sys/types.h, sys/wait.h
    功能：等待任意一个子进程结束，如果任意一个子进程结束了，此函数会回收子进程的资源
    参数：int *wstatus
        进程退出时的状态信息，传入的是一个int类型的地址，传出参数。
    返回值：
        - 成功：返回被回收的子进程的id
        - 失败：-1 (所有的子进程都结束，调用函数失败)

    调用wait函数的进程会被挂起（阻塞），直到它的一个子进程退出或者收到一个不能被忽略的信号时才被唤醒（相当于继续往下执行）
    如果没有子进程了，函数立刻返回，返回-1；如果子进程都已经结束了，也会立即返回，返回-1.

退出信息相关宏函数，wstatus是调用wait()时传入的地址指向的值
    WIFEXITED(status) 非0，进程正常退出
    WEXITSTATUS(status) 如果上宏为真，获取进程退出的状态（exit的参数）
    WIFSIGNALED(status) 非0，进程异常终止
    WTERMSIG(status) 如果上宏为真，获取使进程终止的信号编号 (终端调用 kill -9 进程号，就显示9)
    WIFSTOPPED(status) 非0，进程处于暂停状态
    WSTOPSIG(status) 如果上宏为真，获取使进程暂停的信号的编号
    WIFCONTINUED(status) 非0，进程暂停后已经继续运行

// 获取子进程正常终止值
    WIFEXITED(status) --> 为真 --> 调用 WEXITSTATUS(status) --> 得到子进程退出值

// 获取导致子进程异常终止信号
    WIFSIGNALED(status) --> 为真 --> 调用 WTERMSIG(status) --> 得到导致子进程异常终止的信号编号


pid_t waitpid(pid_t pid, int *wstatus, int options); // sys/types.h, sys/wait.h
    功能：回收指定进程号的子进程，可以设置是否阻塞。
    参数：
        - pid:
            pid > 0 : 某个子进程的pid
            pid = 0 : 回收当前进程组的所有子进程    
            pid = -1 : 回收所有的子进程，相当于 wait()  （最常用）
            pid < -1 : 某个进程组的组id的绝对值，回收指定进程组中的子进程
        - options：设置阻塞或者非阻塞
            0 : 阻塞
            WNOHANG : 非阻塞
        - 返回值：
            > 0 : 返回子进程的id
            = 0 : options = WNOHANG, 表示还有子进程
            = -1 ：错误，或者没有子进程了

wait() 等价于 waitpid(-1, &wstatus, 0);
```
## 案例

```c++
#include <sys/types.h>
#include <sys/wait.h>
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>

int main() {
    pid_t pid;
    for(int i = 0; i < 5; i++) { // 创建5个子进程
        pid = fork();
        if(pid == 0) break;
    }
    if(pid > 0) {// 父进程
        while(1) {
            printf("parent, pid = %d\n", getpid());
            sleep(1);
            int st;
            int ret = waitpid(-1, &st, WNOHANG);
            if(ret == -1) break;
            else if (ret == 0) continue; // 说明还有子进程活着
            else if (ret > 0)
            {
                if(WIFEXITED(st))// 是不是正常退出
                    printf("退出的状态码：%d\n", WEXITSTATUS(st));
                if(WIFSIGNALED(st)) // 是不是异常终止
                    printf("被哪个信号干掉了：%d\n", WTERMSIG(st));
                printf("child die, pid = %d\n", ret);
            }
        }
    } else if (pid == 0){ // 子进程
        while(1) {
            printf("child, pid = %d\n",getpid());    
            sleep(1);       
        }      
        exit(0);
    }
    return 0; 
}
```
# lesson10-29 进程间通信

***进程间通讯概念***

进程是一个独立的资源分配单元，不同进程（这里所说的进程通常指的是用户进程）之间的资源是独立的，没有关联，不能在一个进程中直接访问另一个进程的资源。

但是，进程不是孤立的，不同的进程需要进行信息的交互和状态的传递等，因此需要进程间通信( IPC：Inter Processes Communication )。

进程间通信的目的：
- 数据传输：一个进程需要将它的数据发送给另一个进程。
- 通知事件：一个进程需要向另一个或一组进程发送消息，通知它们发生了某种事件（如进程终止时通知父进程）。
- 资源共享：多个进程之间共享同样的资源。为了做到这一点，需要内核提供互斥和同步机制。
- 进程控制：有些进程希望完全控制另一个进程的执行（如 Debug 进程），此时控制进程希望能够拦截另一个进程的所有陷入和异常，并能够及时知道它的状态改变

***Linux 进程间通信的方式***


![Linux 进程间通信的方式](https://github.com/casey-li/Notes/blob/main/webserver/pic/%E7%AC%AC%E4%BA%8C%E7%AB%A0%E3%80%81Linux%E5%A4%9A%E8%BF%9B%E7%A8%8B%E5%BC%80%E5%8F%91/Linux%20%E8%BF%9B%E7%A8%8B%E9%97%B4%E9%80%9A%E4%BF%A1%E7%9A%84%E6%96%B9%E5%BC%8F.png?raw=true)

***特点***
- 管道：简单
- 信号：开销小
- mmap映射：非血缘关系进程间
- socket（本地套接字）：稳定

## 一、匿名管道

管道也叫无名（匿名）管道，它是是 UNIX 系统 IPC（进程间通信）的最古老形式，所有的 UNIX 系统都支持这种通信机制。

统计一个目录中文件的数目命令：ls | wc –l，为了执行该命令，shell 创建了两个进程来分别执行 ls 和 wc (用于计算文件的行数 `-l` 、字数 `-w` 和字节数 `-c`)

![匿名管道](https://github.com/casey-li/Notes/blob/main/webserver/pic/%E7%AC%AC%E4%BA%8C%E7%AB%A0%E3%80%81Linux%E5%A4%9A%E8%BF%9B%E7%A8%8B%E5%BC%80%E5%8F%91/%E5%8C%BF%E5%90%8D%E7%AE%A1%E9%81%93.png?raw=true)

### 1、管道的特点

管道其实是一个在内核内存中维护的缓冲器，这个缓冲器的存储能力是有限的，不同的操作系统大小不一定相同。

管道拥有文件的特质：读操作、写操作，匿名管道没有文件实体，有名管道有文件实体，但不存储数据，可以按照操作文件的方式对管道进行操作。通过管道传递的数据是顺序的，从管道中读取出来的字节的顺序和它们被写入管道的顺序是完全一样的（管道的数据结构：环形队列）。在管道中的数据的传递方向是单向的，一端用于写入，一端用于读取，管道是半双工的 (对讲机)。

从管道读数据是一次性操作，数据一旦被读走，它就从管道中被抛弃，因此无法使用 lseek() 来随机的访问数据。

匿名管道只能在具有公共祖先的进程（父进程与子进程，或者两个兄弟进程，具有亲缘关系）之间使用。因为fork() 出来的进程的文件描述符表中有文件描述符分别指向了同一个管道的读端以及写端

### 2、匿名管道的使用
```c++
// 创建一个匿名管道，用来进程间通信。在 fork()之前创建管道，这样两个进程对应的才是一个管道
int pipe(int pipefd[2]); // unistd.h
    参数：
        int pipefd[2] 这个数组是一个传出参数。
        pipefd[0] 对应的是管道的读端
        pipefd[1] 对应的是管道的写端
    返回值：
        成功 0, 失败 -1
```

管道默认是阻塞的：如果管道中没有数据，read阻塞，如果管道满了，write阻塞
注意：匿名管道只能用于具有关系的进程之间的通信（父子进程，兄弟进程）

#### 案例：实现 ps aux, 父子进程间通信
```c++      
//子进程： ps aux, 子进程结束后，将数据发送给父进程；父进程：获取数据
//需要 pipe(), execlp(),  子进程将标准输出 stdout_fileno 重定向到管道的写端(dup2)，本来输出到终端，现在输出到管道
int main() {
    int fd[2];
    int ret = pipe(fd);
    if(ret == -1) {
        perror("pipe");
        exit(0);
    }
    pid_t pid = fork();
    if(pid > 0) {
        close(fd[1]); // 关闭写端
        // 从管道中读取
        char buf[1024] = {0};
        int len = -1;
        while((len = read(fd[0], buf, sizeof(buf) - 1)) > 0) {
            printf("%s", buf);
            memset(buf, 0, 1024);
        }
        wait(NULL);
    } else if(pid == 0) {
        close(fd[0]); // 关闭读端
        dup2(fd[1], STDOUT_FILENO); // 文件描述符的重定向 stdout_fileno -> fd[1]
        execlp("ps", "ps", "aux", NULL); // 执行 ps aux，最好循环读，因为数据量可能超过管道容量上限
        perror("execlp");
        exit(0);
    } else {
        perror("fork");
        exit(0);
    }
    return 0;
}
```

查看管道缓冲大小命令: `ulimit –a`
查看管道缓冲大小函数: `long fpathconf(int fd, int name);` 
```c++
//例子：获取管道的大小
int pipefd[2];
int ret = pipe(pipefd);
long size = fpathconf(pipefd[0], _PC_PIPE_BUF);
```

### 3、总结：

- 读管道：
    - 管道中有数据，read返回实际读到的字节数。
    - 管道中无数据：
        - 写端被全部关闭，read返回0（相当于读到文件的末尾）
        - 写端没有完全关闭，read阻塞等待
- 写管道：
    - 管道读端全部被关闭，进程异常终止（进程收到`SIGPIPE`信号）
    - 管道读端没有全部关闭：
        - 管道已满，write阻塞
        - 管道没有满，write将数据写入，并返回实际写入的字节数

## 二、有名管道

匿名管道由于没有名字，只能用于亲缘关系的进程间通信

有名管道（FIFO）提供了一个路径名与之关联，以 FIFO 的文件形式存在于文件系统中，并且其打开方式与打开一个普通文件是一样的。这样不相关的进程就能够彼此通过 FIFO 相互通信，交换数据。与管道一样，FIFO 也有一个写入端和读取端，并且读写顺序是一致的

有名管道 (FIFO) 和匿名管道 (pipe) 有一些特点是相同的，不一样的地方在于：
1. FIFO 在文件系统中作为一个特殊文件存在，但 FIFO 中的内容却存放在内存中。
2. 当使用 FIFO 的进程退出后，FIFO 文件将继续保存在文件系统中以便以后使用。
3. FIFO 有名字，不相关的进程可以通过打开有名管道进行通信。

### 有名管道的使用
```c++
// 1.通过命令创建有名管道
mkfifo 名字

// 2.通过函数创建有名管道
int mkfifo(const char *pathname, mode_t mode); // sys/types.h, sys/stat.h
    参数：
        - pathname: 管道名称的路径
        - mode: 文件的权限 和 open 的 mode 是一样的, 是一个八进制的数
    返回值：成功返回0，失败返回-1，并设置错误号
```
一旦使用 mkfifo 创建了一个 FIFO，就可以使用 open 打开它，常见的文件I/O 函数都可用于 FIFO。FIFO 严格遵循先进先出，同样不支持诸如 lseek() 等文件定位操作。

***有名管道的注意事项：***
1. 一个为只读而打开一个管道的进程会阻塞，直到另外一个进程为只写打开管道
2. 一个为只写而打开一个管道的进程会阻塞，直到另外一个进程为只读打开管道

***总结***
- 读管道：
    - 管道中有数据，read返回实际读到的字节数
    - 管道中无数据：
        - 管道写端被全部关闭，read返回0，（相当于读到文件末尾）
        - 写端没有全部被关闭，read阻塞等待

- 写管道：
    - 管道读端被全部关闭，进行异常终止（收到一个`SIGPIPE`信号）
    - 管道读端没有全部关闭：
        - 管道已经满了，write会阻塞
        - 管道没有满，write将数据写入，并返回实际写入的字节数。

## 三、内存映射

内存映射（Memory-mapped I/O）是将磁盘文件的数据映射到内存，用户通过修改内存就能修改磁盘文件。效率更高，**既可以实现有关系的进程之间的通信也可以实现无关系的进程之间的通信**

### 1、内存映射相关系统调用
```c++
void *mmap(void *addr, size_t length, int prot, int flags, int fd, off_t offset); // sys/mman.h
    - 功能：在调用进程的虚拟地址空间中创建一个映射，将一个文件或者设备的数据映射到内存中
    - 参数：
        - void *addr: NULL, 由内核指定

        - length : 要映射的数据的长度，这个值不能为0。建议使用文件的长度。
                获取文件的长度：stat lseek，若该值小于分页的大小会扩充，至少为分页大小的整数倍

        - prot : 对申请的内存映射区的操作权限
            -PROT_EXEC ：可执行的权限
            -PROT_READ ：读权限
            -PROT_WRITE ：写权限
            -PROT_NONE ：没有权限
            要操作映射内存，必须要有读的权限。
            通常 : PROT_READ、PROT_READ|PROT_WRITE

        - flags :
            - MAP_SHARED : 映射区的数据会自动和磁盘文件进行同步，进程间通信，必须要设置这个选项
            - MAP_PRIVATE ：不同步，内存映射区的数据改变了，对原来的文件不会修改，会重新创建一个新的文件。（copy on write）

        - fd: 需要映射的那个文件的文件描述符
            - 通过open得到，open的是一个磁盘文件
            - 注意：文件的大小不能为0，open指定的权限不能和prot参数有冲突 (prot的权限 <= open 的权限)
                prot: PROT_READ                open:只读/读写 
                prot: PROT_READ | PROT_WRITE   open:读写

        - offset：偏移量，一般不用。必须指定的是4k的整数倍，0表示不偏移

    - 返回值：返回创建的内存的首地址
        失败返回MAP_FAILED，(void *) -1

int munmap(void *addr, size_t length);
    - 功能：释放内存映射
    - 参数：
        - addr : 要释放的内存的首地址
        - length : 要释放的内存的大小，要和mmap函数中的length参数的值一样。
```

### 2、使用内存映射实现进程间通信

1. 有关系的进程（父子进程）
    - 还没有子进程的时候
        - 通过唯一的父进程，先创建内存映射区 (要有磁盘文件，否则先创建)
    - 有了内存映射区以后，创建子进程
    - 父子进程共享创建的内存映射区

2. 没有关系的进程间通信
    - 准备一个大小不是0的磁盘文件
    - 进程1 通过磁盘文件创建内存映射区
        - 得到一个操作这块内存的指针
    - 进程2 通过磁盘文件创建内存映射区
        - 得到一个操作这块内存的指针
    - 使用内存映射区通信

注意：内存映射区通信，是非阻塞。

***思考问题***

1. 如果对`mmap()`的返回值做++操作(`ptr++`), `munmap()`是否能够成功?
```c++
void * ptr = mmap(...);
ptr++;  可以对其进行++操作
munmap(ptr, len);   // 错误,要保存地址
```
2. 如果`open()`时`O_RDONLY`, `mmap()`时prot参数指定`PROT_READ | PROT_WRITE`会怎样?
    错误，返回`MAP_FAILED`，`open()`函数中的权限建议和`prot`参数的权限保持一致或大于等于`prot`的权限

3. 如果文件偏移量为1000会怎样?
    偏移量必须是4K的整数倍，返回`MAP_FAILED`

4. `mmap()`什么情况下会调用失败?
    - 第二个参数：length = 0
    - 第三个参数：prot
        - 只指定了写权限
        - prot `PROT_READ | PROT_WRITE`
    - 第5个参数fd 通过open函数时指定的 `O_RDONLY / O_WRONLY`
    - ...

5. 可以`open()`的时候`O_CREAT`一个新文件来创建映射区吗?
    - 可以的，但是创建的文件的大小如果为0的话，肯定不行
    - 可以对新的文件进行扩展，用 `lseek()` 或 `truncate()`

6. `mmap()`后关闭文件描述符，对`mmap()`映射有没有影响？
```c++
int fd = open("XXX");
mmap(,,,,fd,0);
close(fd); 
// 映射区还存在，创建映射区的fd被关闭，没有任何影响。
```
7. 对`ptr`越界操作会怎样？
    `void * ptr = mmap(NULL, 100,,,,,);`
    越界操作操作的是非法的内存 -> 段错误
    
#### 案例：内存映射做文件拷贝 (可以这么用，但若文件很大内存可能放不下)

思路：
1. 对原始的文件进行内存映射
2. 创建一个新文件（拓展该文件）
3. 把新文件的数据映射到内存中
4. 通过内存拷贝将第一个文件的内存数据拷贝到新的文件内存中
5. 释放资源

正常的内存映射
```c++
int main() {
    int fd = open("english.txt", O_RDWR);
    if(fd == -1) {
        perror("open");
        exit(0);
    }
    int len = lseek(fd, 0, SEEK_END);
    int fd1 = open("cpy.txt", O_RDWR | O_CREAT, 0664);
    if(fd1 == -1) {
        perror("open");
        exit(0);
    }
    truncate("cpy.txt", len); // 拓展该文件
    write(fd1, " ", 1); // 使扩展生效
    void * ptr = mmap(NULL, len, PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);
    void * ptr1 = mmap(NULL, len, PROT_READ | PROT_WRITE, MAP_SHARED, fd1, 0);
    if(ptr == MAP_FAILED || ptr1 == MAP_FAILED) {
        perror("mmap");
        exit(0);
    }
    memcpy(ptr1, ptr, len);
    munmap(ptr1, len);
    munmap(ptr, len);
    close(fd1);
    close(fd);
    return 0;
}
```

匿名映射：不需要文件实体进行一个内存映射，只能有关系的进程之间通信
`flags 新增 MAP_ANONYMOUS, fd为-1`

#### 案例：匿名映射实现进程间通信
```c++
int main() {
    int len = 4096;
    void * ptr = mmap(NULL, len, PROT_READ | PROT_WRITE, MAP_SHARED | MAP_ANONYMOUS, -1, 0);
    if(ptr == MAP_FAILED) {
        perror("mmap");
        exit(0);
    }
    pid_t pid = fork();
    if(pid > 0) {
        strcpy((char *) ptr, "hello, world"); // 父进程写
        wait(NULL);
    }else if(pid == 0) {
        sleep(1);
        printf("%s\n", (char *)ptr); // 子进程读
    }
    int ret = munmap(ptr, len);
    if(ret == -1) {
        perror("munmap");
        exit(0);
    }
    return 0;
}
```
## 四、信号

### 1、简介
信号是 Linux 进程间通信的最古老的方式之一，是事件发生时对进程的通知机制，也称之为软件中断，它是在软件层次上对中断机制的一种模拟，是一种异步通信的方式。

信号可以导致一个正在运行的进程被另一个正在运行的异步进程中断，转而处理某一个突发事件。发往进程的诸多信号，通常都是源于内核。引发内核为进程产生信号的各类事件如下：
1. 对于前台进程，用户可以通过输入特殊的终端字符来给它发送信号。比如输入Ctrl+C 通常会给进程发送一个中断信号。
2. 硬件发生异常，即硬件检测到一个错误条件并通知内核，随即再由内核发送相应信号给相关进程。比如执行一条异常的机器语言指令，诸如被 0 除，或者引用了无法访问的内存区域。
3. 系统状态变化，比如 `alarm` 定时器到期将引起 `SIGALRM` 信号，进程执行的 CPU 时间超限，或者该进程的某个子进程退出。
4. 运行 `kill` 命令或调用 `kill` 函数。

***使用信号的两个主要目的***
- 让进程知道已经发生了一个特定的事情。
- 强迫进程执行它自己代码中的信号处理程序。

***信号的特点：***
1. 简单
2. 不能携带大量信息
3. 满足某个特定条件才发送
4. 优先级比较高

查看系统定义的信号列表：`kill –l `，前 31 个信号为常规信号，其余为实时信号。

***Linux 中几个重要的信号***

| 编号 | 信号名称 | 对应事件 | 默认动作             |
| :-- | :-----------: | :-------: | :----------------: |
| 2  | SIGINT | 当用户按下了`Ctrl + C`组合键时，用户终端向正在运行中的由该终端启动的程序发出此信号 | 终止进程 |
| 3  | SIGQUIT | 用户按下`Ctrl + \`组合键时产生该信号，用户终端向正在运行中的由该终端启动的程序发出些信号 | 终止进程 |
| 9  | SIGKILL | 无条件终止进程。该信号不能被忽略，处理和阻塞 | 终止进程，可以杀死任何正常进程 (不包括僵尸进程) |
| 11  | SIGSEGV | 指示进程进行了无效内存访问(段错误) | 终止进程并产生core文件 |
| 13  | SIGPIPE | `Broken pipe`向一个没有读端的管道写数据 | 终止进程 |
| 17  | SIGCHLD | 子进程结束时，父进程会收到这个信号 | 忽略这个信号 |
| 18  | SIGCONT | 如果进程已停止，则使其继续运行 | 继续/忽略 |
| 19  | SIGSTOP | 停止进程的执行。信号不能被忽略，处理和阻塞 | 为终止进程 |


查看信号的详细信息：`man 7 signal`

***信号的 5 中默认处理动作***
- Term 终止进程
- Ign 当前进程忽略掉这个信号
- Core 终止进程，并生成一个Core文件 (核心软储文件，为了对程序的错误调试)
- Stop 暂停当前进程
- Cont 继续执行当前被暂停的进程

信号的几种状态：产生、未决、递达

**`SIGKILL` 和 `SIGSTOP` 信号不能被捕捉、阻塞或者忽略，只能执行默认动作。**

***生成 core 文件，并定位错误步骤***
1. `ulimit -a` 查看core的大小，若为 0 则默认不生成, `ulimit -c 1024`修改大小为1024
2. 编译程序的时候加上 `-g`, 进行GDB调试
3. 进入GDB调试后，输入 `core-file (core文件名)` 即可显示信号，详细错误以及行号

### 2、信号相关的函数

```c++
// 给任何的进程或者进程组pid, 发送任何的信号 sig
int kill(pid_t pid, int sig);
    - 参数：
        - pid ：
            > 0 : 将信号发送给指定的进程
            = 0 : 将信号发送给当前的进程组内的所有进程
            = -1 : 将信号发送给每一个有权限接收这个信号的进程
            < -1 : 这个pid=某个进程组的ID取反 （-12345）
        - sig : 需要发送的信号的编号或者是宏值，0表示不发送任何信号
    如: kill(getppid(), 9);

// 给当前进程发送信号，等价于 kill(getpid(), sig);
int raise(int sig);
    成功返回 0，失败返回 非0

// 发送SIGABRT信号给当前的进程，杀死当前进程，等价于 kill(getpid(), SIGABRT);
void abort(void);

// 设置定时器，函数调用开始倒计时，当为0的时会给当前的进程发送信号SIGALARM
unsigned int alarm(unsigned int seconds); // unistd.h
    - 参数：
        seconds: 倒计时的时长，单位：秒。如果参数为0，定时器无效（不进行倒计时，不发信号）
            取消一个定时器，通过 alarm(0)
    - 返回值：
        - 之前没有定时器，返回 0
        - 之前有定时器，返回之前的定时器剩余的时间

    - SIGALARM ：默认终止当前的进程，每一个进程都有且只有唯一的一个定时器。
        alarm(10); // 返回0
        过了1秒，再 alarm(5); // 返回9

    alarm() 函数是不阻塞的，不会等之前设置的倒计时走完再设置新的倒计时
    实际的时间 = 内核时间 + 用户时间 + 消耗的时间
    进行文件IO操作的时候比较浪费时间
    定时器，与进程的状态无关（自然定时法）。无论进程处于什么状态，alarm都会计时

// 设置定时器 闹钟。可以替代alarm函数，精度微妙us，可以实现周期性定时
int setitimer(int which, const struct itimerval *new_value, struct itimerval *old_value); //sys/time.h
    - 参数：
        - which : 定时器以什么时间计时
            ITIMER_REAL: 真实时间 (内核时间+用户时间+消耗的时间)，时间到达，发送 SIGALRM， 常用
            ITIMER_VIRTUAL: 用户时间，时间到达，发送 SIGVTALRM
            ITIMER_PROF: 以该进程在用户态和内核态下所消耗的时间来计算，时间到达，发送 SIGPROF

        - new_value: 设置定时器的属性

            struct itimerval {      // 定时器的结构体
                struct timeval it_interval;  // 每个阶段的时间，间隔时间
                struct timeval it_value;     // 延迟多长时间执行定时器
            };

            struct timeval {        // 时间的结构体
                time_t      tv_sec;     //  秒数     
                suseconds_t tv_usec;    //  微秒    
            };
        
        - old_value ：记录上一次的定时的时间参数，一般不使用，指定NULL
    
    - 返回值：
        成功 0，失败 -1 并设置错误号

    setitimer() 同样是非阻塞的
```

### 3、信号捕捉函数

***注意：`SIGKILL `, `SIGSTOP`不能被捕捉，不能被忽略。要在设置定时器之前设置信号捕捉***

通常使用`int sigaction(int signum, const struct sigaction *act, struct sigaction *oldact);`

信号捕捉函数中的临时阻塞信号集指的是**在信号捕捉函数执行过程中，临时阻塞某些信号**
```c++
typedef void (*sighandler_t)(int); // 函数指针

// 设置某个信号的捕捉行为
sighandler_t signal(int signum, sighandler_t handler); // signal.h
    - 参数：
        - signum: 要捕捉的信号
        - handler: 捕捉到信号要如何处理
            - SIG_IGN： 忽略信号
            - SIG_DFL： 使用信号默认的行为
            - 回调函数:  这个函数是内核调用，程序员只负责写，捕捉到信号后如何去处理信号

            回调函数：
                - 需要程序员实现，提前准备好的，函数的类型根据实际需求，看函数指针的定义
                - 不是程序员调用，而是当信号产生，由内核调用
                - 函数指针是实现回调的手段，函数实现之后，将函数名放到函数指针的位置就可以了。

    - 返回值：
        成功，返回上一次注册的信号处理函数的地址。第一次调用返回NULL
        失败，返回SIG_ERR，设置错误号
        
// 检查或者改变信号的处理。信号捕捉
int sigaction(int signum, const struct sigaction *act, struct sigaction *oldact); //signal.h
    - 参数：
        - signum : 需要捕捉的信号的编号或者宏值（信号的名称）
        - act ：捕捉到信号之后的处理动作
        - oldact : 上一次对信号捕捉相关的设置，一般不使用，传递NULL
    - 返回值：
        成功 0, 失败 -1

//正常设置 sa_flags = 0, 清空sa_mask, 再给sa_handler赋值即可
struct sigaction {
    void (*sa_handler)(int); // 函数指针，指向的函数就是信号捕捉到之后的处理函数
    void (*sa_sigaction)(int, siginfo_t *, void *); // 不常用
    sigset_t sa_mask; // 临时阻塞信号集，在信号捕捉函数执行过程中，临时阻塞某些信号。
    // 使用哪一个信号处理对捕捉到的信号进行处理
    int sa_flags; // 0，表示使用sa_handler（*）, SA_SIGINFO表示使用sa_sigaction
    void (*sa_restorer)(void); // 被废弃掉了
};
```
`SA_RESTART` 是 `sigaction` 函数的标志之一，用于设置信号处理期间的系统调用行为 ( `sa.sa_flags |= SA_RESTART`)

当一个进程在执行系统调用（如 `read`、`write` 等）时，如果接收到信号并且**没有设置 `SA_RESTART` 标志**，那么系统调用会被中断，进程需要重新启动系统调用。这意味着系统调用返回 -1，并且 `errno` 被设置为 `EINTR`，表示系统调用被信号中断

如果设置了 `SA_RESTART` 标志，那么在信号处理完成后，系统调用会自动重新启动，而不是中断。这意味着系统调用会在信号处理完成后继续执行，而不会返回 -1，从而减少了中断系统调用的影响

使用 `SA_RESTART` 标志可以简化信号处理程序的逻辑，使系统调用在被信号中断后自动恢复，而无需手动处理中断和重新启动系统调用的逻辑

### 4、信号集

多个信号可使用一个称之为信号集的数据结构来表示，其系统数据类型为 `sigset_t`

在 PCB 中有两个非常重要的信号集（ “阻塞信号集” 和“未决信号集” ），都是内核使用位图机制来实现的，程序员只能借助信号集操作函数来对它们修改
- 信号的 “未决” 是一种状态，指的是从信号的产生到信号被处理前的这一段时间
- 信号的 “阻塞” 是一个开关动作，指的是**阻止信号被处理，但不是阻止信号产生**

信号的阻塞就是让系统暂时保留信号留待以后发送，防止信号打断敏感的操作

***典型的流程***
1. 用户通过键盘 `Ctrl + C`, 产生2号信号`SIGINT` (信号被创建)

2. 信号产生但是没有被处理 （未决）
    - 内核将所有的没有被处理的信号存储在“未决信号集”中 (标志位为0说明信号不是未决状态，为1说明信号处于未决状态)
    - `SIGINT`信号状态被存储在第二个标志位上，置其为1

3. 未决状态的信号在被处理之前先和“阻塞信号集”进行比较
    - 阻塞信号集默认不阻塞任何的信号
    - 如果想要阻塞某些信号需要用户调用系统的API
    - 如果没有阻塞，这个信号就被处理
    - 如果阻塞了，这个信号就继续处于未决状态，直到阻塞解除，这个信号就被处理

#### 信号集相关的函数

1、操作自定义的信号集：
```c++
// 清空信号集中的数据,将信号集中的所有的标志位置为0
int sigemptyset(sigset_t *set);
    - 参数：set,传出参数，需要操作的信号集
    - 返回值：成功返回0， 失败返回-1

// 将信号集中的所有的标志位置为1
int sigfillset(sigset_t *set);
    - 返回值：成功返回0， 失败返回-1

// 设置信号集中的某一个信号对应的标志位为1，表示阻塞这个信号
int sigaddset(sigset_t *set, int signum);
    - 参数：
        - set：传出参数，需要操作的信号集
        - signum：需要设置阻塞的那个信号
    - 返回值：成功返回0， 失败返回-1

// 设置信号集中的某一个信号对应的标志位为0，表示不阻塞这个信号
int sigdelset(sigset_t *set, int signum);
    - 返回值：成功返回0， 失败返回-1

// 判断某个信号是否阻塞
int sigismember(const sigset_t *set, int signum);
    - 参数：
        - set：需要操作的信号集
        - signum：需要判断的那个信号
    - 返回值：
        1 ： signum被阻塞
        0 ： signum不阻塞
        -1 ： 失败
```

2. 对内核信号集的操作 (**未决不能修改**)

```c++
// 将自定义信号集中的数据设置到内核中（设置阻塞，解除阻塞，替换）
int sigprocmask(int how, const sigset_t *set, sigset_t *oldset);
    - 参数：
        - how : 如何对内核阻塞信号集进行处理
            SIG_BLOCK: 将用户设置的阻塞信号集添加到内核中，内核中原来的数据不变
                假设内核中默认的阻塞信号集是mask， mask | set
            SIG_UNBLOCK: 根据用户设置的数据，对内核中的数据进行解除阻塞
                mask &= ~set
            SIG_SETMASK:覆盖内核中原来的值
        
        - set ：已经初始化好的用户自定义的信号集
        - oldset : 保存设置之前的内核中的阻塞信号集的状态，可以是 NULL
    - 返回值：
        成功：0，失败：-1 并设置错误号：EFAULT、EINVAL

// 获取内核中的未决信号集
int sigpending(sigset_t *set);
    - 参数：set,传出参数，保存的是内核中的未决信号集中的信息。
```

***内核实现信号捕捉的过程***

![内核实现信号捕捉的过程](https://github.com/casey-li/Notes/blob/main/webserver/pic/%E7%AC%AC%E4%BA%8C%E7%AB%A0%E3%80%81Linux%E5%A4%9A%E8%BF%9B%E7%A8%8B%E5%BC%80%E5%8F%91/%E5%86%85%E6%A0%B8%E5%AE%9E%E7%8E%B0%E4%BF%A1%E5%8F%B7%E6%8D%95%E6%8D%89%E7%9A%84%E8%BF%87%E7%A8%8B.png?raw=true)

#### SIGCHLD信号

`SIGCHLD`信号产生的条件：
1. 子进程终止时
2. 子进程接收到`SIGSTOP`信号停止时
3. 子进程处在停止态，接受到`SIGCONT`后唤醒时

以上三种条件都会给父进程发送`SIGCHLD`信号，父进程默认会忽略该信号

#### 案例：使用`SIGCHLD`信号解决僵尸进程的问题。
```c++
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <signal.h>
#include <sys/wait.h>

// 原来是父进程调用 wait() 或者 waitpid() 来回收子进程的资源，但是注意 wait() 是阻塞的。更优的方法是父进程捕获SIGCHLD信号后再调用wait() 或者 waitpid()
void myFun(int num) {
    printf("捕捉到的信号 ：%d\n", num);
    // 回收子进程PCB的资源
    while(1) 
    {
        int ret = waitpid(-1, NULL, WNOHANG);
        if(ret > 0) printf("child die , pid = %d\n", ret);
        else if(ret == 0) break; // 说明还有子进程活着
        else if(ret == -1) break; // 没有子进程
    }
}

int main() {
    // 提前设置好阻塞信号集，阻塞SIGCHLD，因为有可能子进程很快结束，父进程还没有注册完信号捕捉
    sigset_t set;
    sigemptyset(&set);
    sigaddset(&set, SIGCHLD);
    sigprocmask(SIG_BLOCK, &set, NULL);
    // 创建一些子进程
    pid_t pid;
    for(int i = 0; i < 20; i++) 
    {
        pid = fork();
        if(pid == 0) break; //避免子进程再创建子进程
    }
    if(pid > 0) 
    {
        // 捕捉子进程死亡时发送的SIGCHLD信号
        struct sigaction act;
        act.sa_flags = 0;
        act.sa_handler = myFun;
        sigemptyset(&act.sa_mask);
        sigaction(SIGCHLD, &act, NULL);
        // 注册完信号捕捉以后，解除阻塞
        sigprocmask(SIG_UNBLOCK, &set, NULL);
        while(1) 
        {
            printf("parent process pid : %d\n", getpid());
            sleep(2);
        }
    } 
    else if( pid == 0) printf("child process pid : %d\n", getpid());
    return 0;
}
```

## 五、共享内存

共享内存允许两个或者多个进程共享物理内存的同一块区域（通常被称为段），因此这种 IPC 机制无需内核介入。所有需要做的就是让一个进程将数据复制进共享内存中，并且这部分数据会对其他所有共享同一个段的进程可用。

与管道等要求发送进程将数据从用户空间的缓冲区复制进内核内存和接收进程将数据从内核内存复制进用户空间的缓冲区的做法相比，这种 IPC 技术的速度更快。

### 1、共享内存使用步骤

1. 调用 `shmget()` 创建一个新的共享内存段的标识符或得到一个由其他进程创建的共享内存段的标识符

2. 使用 `shmat()`来附上共享内存段，即使该段成为调用进程的虚拟内存的一部分。为引用这块共享内存，程序需要使用它返回的 `addr` 值，即指向进程的虚拟地址空间中该共享内存段的起点的指针。

3. 调用 `shmdt()` 来分离共享内存段。在这个调用之后，进程就无法再引用这块共享内存了。这一步是可选的，并且在进程终止时会自动完成这一步。

4. 调用 `shmctl()` 来删除共享内存段。只有当当前所有附加内存段的进程都与之分离之后内存段才会销毁。只有一个进程需要执行这一步。

### 2、共享内存操作函数

```c++
// 创建一个新的共享内存段，或者获取一个既有的共享内存段的标识。新创建的内存段中的数据都会被初始化为0
int shmget(key_t key, size_t size, int shmflg);
    - 参数：
        - key: key_t类型是一个整形，通过这个找到或者创建一个共享内存。一般使用16进制表示，非0值
        - size: 共享内存的大小
        - shmflg: 属性
            - 访问权限
            - 附加属性：创建/判断共享内存是不是存在
                - 创建：IPC_CREAT
                - 判断共享内存是否存在： IPC_EXCL , 需要和IPC_CREAT一起使用
                    IPC_CREAT | IPC_EXCL | 0664
        - 返回值：
            失败：-1 并设置错误号
            成功：>0 返回共享内存的引用的ID，后面操作共享内存都是通过这个值。

// 和当前的进程进行关联
void *shmat(int shmid, const void *shmaddr, int shmflg);
    - 参数：
        - shmid : 共享内存的标识（ID）,由shmget返回值获取
        - shmaddr: 申请的共享内存的起始地址，指定NULL，内核指定
        - shmflg : 对共享内存的操作
            - 读 ： SHM_RDONLY, 必须要有读权限
            - 读写： 0
    - 返回值：
        成功：返回共享内存的首（起始）地址。  失败(void *) -1

// 解除当前进程和共享内存的关联
int shmdt(const void *shmaddr);
    - shmaddr：共享内存的首地址
    - 返回值：成功 0， 失败 -1

// 对共享内存进行操作。删除共享内存，共享内存要删除才会消失，创建共享内存的进程被销毁了对共享内存没有任何影响！
int shmctl(int shmid, int cmd, struct shmid_ds *buf);
    - 参数：
        - shmid: 共享内存的ID

        - cmd : 要做的操作
            - IPC_STAT : 获取共享内存的当前的状态
            - IPC_SET : 设置共享内存的状态
            - IPC_RMID: 标记共享内存被销毁，只是标记不一定真正删除，只有关联的进程数为0时才真正被删除

        - buf：需要设置或者获取的共享内存的属性信息
            - IPC_STAT : buf存储数据
            - IPC_SET : buf中需要初始化数据，设置到内核中
            - IPC_RMID : 没有用，NULL

// 根据指定的路径名，和int值，生成一个共享内存的key
key_t ftok(const char *pathname, int proj_id);
    - 参数：
        - pathname:指定一个存在的路径
            如: /home/nowcoder/Linux/a.txt
        - proj_id: int类型的值，但是这系统调用只会使用其中的1个字节
                范围 ： 0-255  一般指定一个字符 'a'
    只要传递的参数相同，生成的key就是一样的
```

- Q1：操作系统如何知道一块共享内存被多少个进程关联？

共享内存维护了一个结构体struct shmid_ds 这个结构体中有一个成员 shm_nattch，它记录了关联的进程个数

- Q2：可不可以对共享内存进行多次删除 shmctl

可以，因为 `shmctl()` 只是标记删除共享内存，不是直接删除。只有当和共享内存关联的进程数为0时共享内存才被真正删除。
此外，当共享内存的key为0的时候，表示共享内存被标记删除了，如果一个进程和共享内存取消关联，那么这个进程就不能继续操作这个共享内存。也不能进行关联。

- Q3：共享内存和内存映射的区别
    1. 共享内存可以直接创建，内存映射需要磁盘文件（匿名映射除外）
    2. 共享内存效果更高
    3. 内存
        - 共享内存所有的进程操作的是同一块共享内存。
        - 内存映射，每个进程在自己的虚拟地址空间中有一个独立的内存。
    4. 数据安全
        - 进程突然退出
            - 共享内存还存在 (必须标记为删除并且连接数为0才会被删除)
            - 内存映射区消失
        - 运行进程的电脑死机，宕机了
            - 数据存在在共享内存中，没有了
            - 内存映射区的数据 ，由于磁盘文件中的数据还在，所以内存映射区的数据还存在。
    5. 生命周期
        - 内存映射区：进程退出，内存映射区销毁
        - 共享内存：进程退出，共享内存还在，标记删除（所有的关联的进程数为0），或者关机
            如果一个进程退出，会自动和共享内存进行取消关联。

### 3、共享内存操作命令
```c++
ipcs 用法
    ipcs -a // 打印当前系统中所有的进程间通信方式的信息
    ipcs -m // 打印出使用共享内存进行进程间通信的信息
    ipcs -q // 打印出使用消息队列进行进程间通信的信息
    ipcs -s // 打印出使用信号进行进程间通信的信息
ipcrm 用法
    ipcrm -M shmkey // 移除用shmkey创建的共享内存段
    ipcrm -m shmid // 移除用shmid标识的共享内存段
    ipcrm -Q msgkey // 移除用msqkey创建的消息队列
    ipcrm -q msqid // 移除用msqid标识的消息队列
    ipcrm -S semkey // 移除用semkey创建的信号
    ipcrm -s semid // 移除用semid标识的信号
```

# lesson30-31 守护进程

## 一、概念

***1、终端***

在 UNIX 系统中，用户通过终端登录系统后得到一个 `shell` 进程，这个终端成为 `shell` 进程的控制终端。控制终端是保存在 PCB 中的信息，而 `fork()` 会复制 PCB 中的信息，因此由 `shell` 进程启动的其它进程的控制终端也是这个终端。

默认情况下（没有重定向），每个进程的标准输入、标准输出和标准错误输出都指向控制终端，进程从标准输入读也就是读用户的键盘输入，进程往标准输出或标准错误输出写也就是输出到显示器上。

在控制终端输入一些特殊的控制键可以给前台进程发信号，例如 `Ctrl + C` 会产生 `SIGINT` 信号，`Ctrl + \` 会产生 `SIGQUIT` 信号。

***2、进程组***

进程组和会话在进程之间形成了一种两级层次关系：进程组是一组相关进程的集合，会话是一组相关进程组的集合。进程组和会话是为支持 shell 作业控制而定义的抽象概念，用户通过 shell 能够交互式地在前台或后台运行命令。

进程组由共享同一进程组标识符（PGID）的进程组成。一个进程组拥有一个进程组首进程，该进程是创建该组的进程，其进程 ID 为该进程组的 ID，新进程会继承其父进程所属的进程组 ID。

进程组拥有一个生命周期，其开始时间为首进程创建组的时刻，结束时间为最后一个成员进程退出组的时刻。一个进程可能会因为终止或加入了另外一个进程组而退出进程组。进程组首进程无需是最后一个离开进程组的成员。

***3、会话***

会话是一组进程组的集合。会话首进程是创建该新会话的进程，其进程 ID 会成为会话 ID。新进程会继承其父进程的会话 ID。

一个会话中的所有进程共享单个控制终端。控制终端会在会话首进程首次打开一个终端设备时被建立。一个终端最多可能会成为一个会话的控制终端。

在任一时刻，会话中只有一个进程组会成为终端的前台进程组。只有前台进程组中的进程才能从控制终端中读取输入。当用户在控制终端中输入终端字符生成信号后，该信号会被发送到前台进程组中的所有成员。
 
当控制终端的连接建立起来之后，会话首进程会成为该终端的控制进程。

## 二、进程组、会话操作函数

```c++
pid_t getpgrp(void); // 获取进程所属的进程组ID
pid_t getpgid(pid_t pid);
int setpgid(pid_t pid, pid_t pgid);
pid_t getsid(pid_t pid);
pid_t setsid(void);
```

## 三、守护进程
守护进程（Daemon Process），是Linux 中的后台服务进程。它是一个生存期较长的进程，通常独立于控制终端并且周期性地执行某种任务或等待处理某些发生的事件。一般采用以 d 结尾的名字。

守护进程具备下列特征：
- 生命周期很长，守护进程会在系统启动的时候被创建并一直运行直至系统被关闭。
- 它在后台运行并且不拥有控制终端。没有控制终端确保了内核永远不会为守护进程自动生成任何控制信号以及终端相关的信号（如 `SIGINT`、`SIGQUIT`）。因此不能通过键盘输入来杀死该进程，可以使用`kill -9`
- Linux 的大多数服务器就是用守护进程实现的。比如，Internet 服务器 `inetd`，Web 服务器 `httpd` 等。

### 守护进程的创建步骤

1. 执行 `fork()`，之后父进程退出(避免结束后终端给终端提示符)，子进程继续执行。(这样子进程不是进程组的首进程，以成功调用`setsid()`，父进程调用的话存在冲突的问题)
2. 子进程调用 `setsid()` 开启一个新会话。(创建进程组以及会话，新会话是没有控制终端的，若父进程调用`setsid()`的话，创建的进程组的id仍等于父进程的id，此时会有两个组的id相同，冲突，所以让子进程创建。那么组id和会话id都等于子进程的id)
3. 清除进程的 `umask` 以确保当守护进程创建文件和目录时拥有所需的权限。
4. 修改进程的当前工作目录，通常会改为根目录（/）。
5. 关闭守护进程从其父进程继承而来的所有打开着的文件描述符。
6. 在关闭了文件描述符0、1、2之后，守护进程通常会打开/dev/null 并使用 `dup2()` 使所有这些描述符指向这个设备。
7. 核心业务逻辑

(1). 完成第一步后就会在shell终端里造成一程序已经运行完毕的假象。之后的所有后续工作都在子进程中完成，而用户在shell终端里则可以执行其他的命令，从而在形式上做到了与控制终端的脱离。此外原先的子进程会变为孤儿进程，进而变为`init`进程的子进程
(2). 原先子进程复制了父进程的PCB信息，包括父进程的会话期、进程组和控制终端等，即还不是真正意义上的独立。因此子进程调用`setsid()`可以摆脱原会话、原进程组、原控制终端的控制。
(3). `fork()`创建的子进程继承了父进程的掩码、工作目录、打开的文件描述符，为了增强守护进程的灵活性，重新设置掩码以及工作目录；此外守护进程可能永远也不会对打开的文件进行读写，但是它们仍会消耗资源，因此全部关闭。守护进程已经与控制终端断开了连接，因此0,1,2这三个文件描述符也失去了存在的价值，关闭！


### 案例：创建守护进程，每隔2s获取一下系统时间，将这个时间写入到磁盘文件中。
```c++
#include <stdio.h>
#include <sys/stat.h>
#include <sys/types.h>
#include <unistd.h>
#include <fcntl.h>
#include <sys/time.h>
#include <signal.h>
#include <time.h>
#include <stdlib.h>
#include <string.h>

void work(int num) { // 捕捉到信号之后，获取系统时间，写入磁盘文件
    time_t tm = time(NULL);
    struct tm * loc = localtime(&tm);
    char * str = asctime(loc);
    int fd = open("time.txt", O_RDWR | O_CREAT | O_APPEND, 0664);
    write(fd ,str, strlen(str));
    close(fd);
}

int main() {
    // 1.创建子进程，退出父进程
    pid_t pid = fork();
    if(pid > 0) exit(0);
    // 2.将子进程重新创建一个会话
    setsid();
    // 3.设置掩码
    umask(022);
    // 4.更改工作目录
    chdir("/home/nowcoder/");
    // 5. 关闭、重定向文件描述符
    int fd = open("/dev/null", O_RDWR);
    dup2(fd, STDIN_FILENO);
    dup2(fd, STDOUT_FILENO);
    dup2(fd, STDERR_FILENO);
    // 6.业务逻辑
    // 捕捉定时信号
    struct sigaction act;
    act.sa_flags = 0;
    act.sa_handler = work;
    sigemptyset(&act.sa_mask);
    sigaction(SIGALRM, &act, NULL);
    struct itimerval val;
    val.it_value.tv_sec = 2;
    val.it_value.tv_usec = 0;
    val.it_interval.tv_sec = 2;
    val.it_interval.tv_usec = 0;
    // 创建定时器
    setitimer(ITIMER_REAL, &val, NULL);
    // 不让进程结束
    while(1) sleep(10);
    return 0;
}
```