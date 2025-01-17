## 什么是并发
最简单和最基本的并发,是指两个或更多独立的活动同时发生
### 计算机系统中的并发
1. 单核处理器运行多任务时进行任务切换（操作系统要进行上下文切换，保存 CPU 的状态和指令指针，并加载新任务的状态信息，这需要时间开销）
2. 多处理器、多核处理器以及二者的结合，属于硬件并发
### 并发的方法
1. 多进程并发
   1. 同一数据的内存地址在不同的进程中可能不相同
   2. 进程间通信的方法：信号，套接字，文件，管道等等
   3. 进程间通信的缺点：建立起来复杂，速度慢
   4. 运行多个进程有固有的开销：启动进程需要时间，操作系统需要内部资源来管理进程
   5. 多进程并发的优点：操作系统在进程间附加的保护和更高级别的通信机制，意味着可以更容易编写安全的并发代码
   6. 多进程并发的优点：可以在不同的机器上运行独立的进程，通过网络连接起来。
2. 多线程并发
   1. 在单个进程中运行多个线程
   2. 进程中的所有线程都共享相同的 地址空间，并且大部分数据可以被所有线程直接访问——全局变量仍然是全局的，指针、 对象的引用或数据可以在线程之间传递。
   3. 同一进程中的线程之间可以通过共享内存进行通信
   4. 多线程并发的开销比多进程并发更小
   5. 多线程并发缺少对数据的保护，所以程序员需要自己做额外的工作来确保多个线程访问同一数据时看到的数据视图是一致的。
   6. 多线程的开销小，如果妥善处理共享内存的问题，则多线程是更好的并发实现方式。况且C++标准也没有支持进程间通信。
### 并发与并行
这两个术语的概念大部分是重叠的，并且经常混用。二者仅存在细微区别：
1. 并行更加面向性能。主要关注使用可用硬件来 提高批量数据处理的性能
2. 并发主要强调 关注点分离（separation of concerns）或响应能力

## 为什么使用并发？
### 关注点分离
将相关的代码聚合同时将无关的代码 分离，可以使程序更易于理解和测试，从而更不容易出 bug。
你可以使用并发来分离不同 的功能区域，即使在这些不同区域上的操作需要同时发生；
如果不显式地使用并发，那要 么编写一个任务切换框架，要么在操作中主动地调用一段无关区域的代码。
### 性能：任务和数据的并行
1. 任务并行：将单个任务分成几部分， 且各自并行运行，从而降低总运行时间。
2. 数据并行：对数据进行拆分，每个线程在不同的数据部分上执行相同的操作。
### 何时不使用并发
唯一原因： **收益比不上成本**

1. 编写和维护多线程代码 会直接产生脑力成本
2. 额外的复杂性也可能会引发更多的 bug
3. 性能增益可能会小于预期；启动线程时存在固定开销，因为操作系统需要分配相关的内核资源和堆栈空间，然后把新线程添加给调度器，这都需要时间。额外开销可能比并发节省的开销更多。
4. 每个线程需要独立的堆栈空间（一般为 1MB），而 32 位处理器的可用内存空间为 4GB, 也就是说 4096 个线程将会用尽所有地址空间，不会给代码、静态数据或者堆数据留有任 何空间
5. 多线程的上下文切换也会耗费时间，使用多线程时需要考虑硬件支持多少个线程同时运行。

## C++中的并发和多线程
### C++多线程历史
1. C++98(1998)标准不承认线程的存在，内存模型也没有正式定义，在缺少编译器相关扩展的情况下，没办法编写多线程应用程序。
2. 随着多线程 C API 的普及——比如 POSIX C 标准和 Microsoft Windows API，很多 C++编译器供应商通过各种平 台相关扩展来支持多线程。此时只能使用平台相关的 C 语言 API来实现多线程。
3. 像 MFC这样的应用框架，以及 Boost 和 ACE 这样的已积累了各种 类的通用 C++库，它们封装了底层平台相关的 API，提供简化任务的高级多线程设施。这些库之间有一个共识：互斥锁使用 RAII 机制来保证退出作用域时解锁互斥锁。
### C++11 标准对并发的支持
1. 线程感知内存模型
2. 管理线程(参见第 2 章)
3. 保护共享数据(参见第 3 章)
4. 线程间 同步操作(参见第 4 章)
5. 底层原子操作(参见第 5 章)

C++ 11 线程库用到了 Boost 库中的许多实现，相关类有着相同名称和结构， Boost 线程库也配合着 C++标准在许多方面做出改变。
### C++14 和 C++17 对并发和并行的更多支持
1. C++14: 只添加了一个新的互斥锁类型  [shared_lock](https://zh.cppreference.com/w/cpp/thread/shared_lock)，用于保护共享数据
2. C++17: 加入了一整套并行算法
### C++线程库的效率
1. C++ 标准中的并发支持库对底层设施进行了抽象与封装，这会带来额外的开销，亦称作：抽象惩罚(abstraction penalty)。
2. C++标准委员会在设计标准线程库时，一个目标就是使得抽象惩罚极低。
3. C++标准委员会的另一个目标是为了达到终极性能，需要确保 C++能给那些期望与硬件 打交道的程序员，提供足够多的的底层设施。

