* C++ 11 标准加入了[并发支持库](https://zh.cppreference.com/w/cpp/thread), 包含线程、原子操作、互斥、条件变量和 future 的内建支持。 
此后要用 C++ 写多线程并发的程序, 不再需要调用平台相关的 API, 如 [Windows API](https://learn.microsoft.com/zh-cn/windows/win32/procthread/multiple-threads) 和 [POSIX thread (pthread)](https://www.cs.cmu.edu/afs/cs/academic/class/15492-f07/www/pthreads.html).

* C++11 引入了 Boost 线程库作为标准线程库，作者 Anthony Williams 为介绍其特性，于 2012 年出版了 *[C++ Concurrency in Action](https://book.douban.com/subject/4130141/)* 一书，并顺应 C++17 于 2019 年 2 月出版了[第二版](https://book.douban.com/subject/27036085/)。*[C++ Concurrency in Action 2ed](https://learning.oreilly.com/library/view/c-concurrency-in/9781617294693/)* 前五章介绍了[线程支持库](https://en.cppreference.com/w/cpp/thread)的基本用法，后六章从实践角度介绍了并发编程的设计思想，相比第一版多介绍了一些 C++17 特性，如 [std::scoped_lock](https://en.cppreference.com/w/cpp/thread/scoped_lock)、[std::shared_mutex](https://en.cppreference.com/w/cpp/thread/shared_mutex)，并多出一章（第十章）介绍 [C++17 标准库并行算法](https://en.cppreference.com/w/cpp/header/execution)，此外个人会在相应处补充 C++20 相关特性，如 [std::jthread](https://en.cppreference.com/w/cpp/thread/jthread)、[std::counting_semaphore](https://en.cppreference.com/w/cpp/thread/counting_semaphore)、[std::barrier](https://en.cppreference.com/w/cpp/thread/barrier)、[std::latch](https://en.cppreference.com/w/cpp/thread/latch) 等。阅读本书前可参考 [Andrew S. Tanenbaum](https://en.wikipedia.org/wiki/Andrew_S._Tanenbaum) 的 [*Modern Operating Systems*](https://book.douban.com/subject/25864553/)，预备操作系统的基础知识（[进程与线程](reference/processes_and_threads.html)、[死锁](reference/deadlocks.html)、[内存管理](reference/memory_management.html)、[文件系统](reference/file_systems.html)、[I/O](reference/IO.html) 等）。此为个人笔记，仅供参考，更详细内容见[原书](https://learning.oreilly.com/library/view/c-concurrency-in/9781617294693/)。

参考资料  
- [1] Anthony Williams. C++ Cocurrency In Action[M]，Second Edition. 王高飞译
- [2] [c++ 11 线程池---完全使用c++ 11新特性](https://www.cnblogs.com/microDeLe/p/16010882.html)
- [3] https://github.com/progschj/ThreadPool
- [4] https://github.com/lzpong/threadpool
- [5] [C++ std::thread - C++ 11 标准库 | 菜鸟教程](https://www.runoob.com/w3cnote/cpp-std-thread.html)
- [C++ 多线程 - Windows | 菜鸟教程](https://www.runoob.com/w3cnote/cpp-multithread-demo.html)
- [C++ 多线程 -  POSIX | 菜鸟教程](https://www.runoob.com/cplusplus/cpp-multithreading.html)
- [C++ Cocurrency In Action 第二版配套源码 anthonywilliams/ccia_code_samples | Github](https://github.com/anthonywilliams/ccia_code_samples)
- [《C++ 并发编程指南》傅海平 | Github](https://github.com/forhappy/Cplusplus-Concurrency-In-Practice)


## [线程支持库](https://en.cppreference.com/w/cpp/thread)
1. [C++并发概述](01_hello_world_of_concurrency_in_cpp.md)
2. [线程管理（Managing thread）](02_managing_thread.html)：[\<thread\>](https://en.cppreference.com/w/cpp/header/thread)
3. [线程间共享数据（Sharing data between thread）](02_sharing_data_between_thread.html)：[\<mutex\>](https://en.cppreference.com/w/cpp/header/mutex)、[\<shared_mutex\>](https://en.cppreference.com/w/cpp/header/shared_mutex)
4. [同步并发操作（Synchronizing concurrent operation）](03_synchronizing_concurrent_operation.html)：[\<condition_variable\>](https://en.cppreference.com/w/cpp/header/condition_variable)、[\<semaphore\>](https://en.cppreference.com/w/cpp/header/semaphore)、[\<barrier\>](https://en.cppreference.com/w/cpp/header/barrier)、[\<latch\>](https://en.cppreference.com/w/cpp/header/latch)、[\<future\>](https://en.cppreference.com/w/cpp/header/future)、[\<chrono\>](https://en.cppreference.com/w/cpp/header/chrono)、[\<ratio\>](https://en.cppreference.com/w/cpp/header/ratio)
5. [C++ 内存模型和基于原子类型的操作（The C++ memory model and operations on atomic type）](04_the_cpp_memory_model_and_operations_on_atomic_type.html)：[\<atomic\>](https://en.cppreference.com/w/cpp/header/atomic)

## 并发编程实践

6. [基于锁的并发数据结构的设计（Designing lock-based concurrent data structure）](05_designing_lock_based_concurrent_data_structure.html)
7. [无锁并发数据结构的设计（Designing lock-free concurrent data structure）](06_designing_lock_free_concurrent_data_structure.html)
8. [并发代码的设计（Designing concurrent code）](07_designing_concurrent_code.html)
9. [高级线程管理（Advanced thread management）](08_advanced_thread_management.html)
10. [并行算法（Parallel algorithm）](09_parallel_algorithm.html)：[\<execution\>](https://en.cppreference.com/w/cpp/header/execution)
11. [多线程应用的测试与调试（Testing and debugging multithreaded application）](10_testing_and_debugging_multithreaded_application.html)

## 附录
1. [C++ 11 语言特性参考](Appx_A_brief_reference_for_some_cpp11_language_features.md)

## 标准库相关头文件

|头文件|说明|
|:-:|:-:|
|[\<thread\>](https://en.cppreference.com/w/cpp/header/thread)、[\<stop_token\>](https://en.cppreference.com/w/cpp/header/stop_token)|线程|
|[\<mutex\>](https://en.cppreference.com/w/cpp/header/mutex)、[\<shared_mutex\>](https://en.cppreference.com/w/cpp/header/shared_mutex)|锁|
|[\<condition_variable\>](https://en.cppreference.com/w/cpp/header/condition_variable)|条件变量|
|[\<semaphore\>](https://en.cppreference.com/w/cpp/header/semaphore)|信号量|
|[\<barrier\>](https://en.cppreference.com/w/cpp/header/barrier)、[\<latch\>](https://en.cppreference.com/w/cpp/header/latch)|屏障|
|[\<future\>](https://en.cppreference.com/w/cpp/header/future)|异步处理的结果|
|[\<chrono\>](https://en.cppreference.com/w/cpp/header/chrono)|时钟|
|[\<ratio\>](https://en.cppreference.com/w/cpp/header/ratio)|编译期有理数算数|
|[\<atomic\>](https://en.cppreference.com/w/cpp/header/atomic)|原子类型和原子操作|
|[\<execution\>](https://en.cppreference.com/w/cpp/header/execution)|标准库算法执行策略|

## 并发库对比

### [C++11 Thread](https://en.cppreference.com/w/cpp/thread)

|特性|API|
|:-:|:-:|
|thread|[std::thread](https://en.cppreference.com/w/cpp/thread/thread)|
|mutex|[std::mutex](https://en.cppreference.com/w/cpp/thread/mutex)、[std::lock_guard](https://en.cppreference.com/w/cpp/thread/lock_guard)、[std::unique_lock](https://en.cppreference.com/w/cpp/thread/unique_lock)|
|condition variable|[std::condition_variable](https://en.cppreference.com/w/cpp/thread/condition_variable)、[std::condition_variable_any](https://en.cppreference.com/w/cpp/thread/condition_variable_any)|
|atomic|[std::atomic](https://en.cppreference.com/w/cpp/atomic/atomic)、[std::atomic_thread_fence](https://en.cppreference.com/w/cpp/atomic/atomic_thread_fence)|
|future|[std::future](https://en.cppreference.com/w/cpp/thread/future)、[std::shared_future](https://en.cppreference.com/w/cpp/thread/shared_future)|
|interruption|无|

### [Boost Thread](https://www.boost.org/doc/libs/1_80_0/doc/html/thread.html)

|特性|API|
|:-:|:-:|
|thread|[boost::thread](https://www.boost.org/doc/libs/1_80_0/doc/html/thread/thread_management.html#thread.thread_management.thread)|
|mutex|[boost::mutex](https://www.boost.org/doc/libs/1_80_0/doc/html/thread/synchronization.html#thread.synchronization.mutex_types.mutex)、[boost::lock_guard](https://www.boost.org/doc/libs/1_80_0/doc/html/thread/synchronization.html#thread.synchronization.lock_guard.lock_guard)、[boost::unique_lock](https://www.boost.org/doc/libs/1_80_0/doc/html/thread/synchronization.html#thread.synchronization.locks.unique_lock)|
|condition variable|[boost::condition_variable](https://www.boost.org/doc/libs/1_80_0/doc/html/thread/synchronization.html#thread.synchronization.condvar_ref.condition_variable)、[boost::condition_variable_any](https://www.boost.org/doc/libs/1_80_0/doc/html/thread/synchronization.html#thread.synchronization.condvar_ref.condition_variable_any)|
|atomic|无|
|future|[boost::future](https://www.boost.org/doc/libs/1_80_0/doc/html/thread/synchronization.html#thread.synchronization.futures.reference.unique_future)、[boost::shared_future](https://www.boost.org/doc/libs/1_80_0/doc/html/thread/synchronization.html#thread.synchronization.futures.reference.shared_future)|
|interruption|[thread::interrupt](https://www.boost.org/doc/libs/1_80_0/doc/html/thread/thread_management.html#thread.thread_management.thread.interrupt)|

### [POSIX Thread](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/pthread.h.html)

|特性|API|
|:-:|:-:|
|thread|[pthread_create](http://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_create.html)、[pthread_detach](http://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_detach.html#)、[pthread_join](http://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_join.html#)|
|mutex|[pthread_mutex_lock、pthread_mutex_unlock](http://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_mutex_lock.html)|
|condition variable|[pthread_cond_wait](http://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_cond_wait.html)、[pthread_cond_signal](https://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_cond_signal.html)|
|atomic|无|
|future|无|
|interruption|[pthread_cancel](http://pubs.opengroup.org/onlinepubs/9699919799/functions/pthread_cancel.html)|

### [Java Thread](https://docs.oracle.com/en/java/javase/19/docs/api/java.base/java/lang/Thread.html)

|特性|API|
|:-:|:-:|
|thread|[java.lang.Thread](https://docs.oracle.com/en/java/javase/19/docs/api/java.base/java/lang/Thread.html)|
|mutex|[synchronized blocks](http://tutorials.jenkov.com/java-concurrency/synchronized.html)|
|condition variable|[java.lang.Object.wait](https://docs.oracle.com/en/java/javase/19/docs/api/java.base/java/lang/Object.html#wait())、[java.lang.Object.notify](https://docs.oracle.com/en/java/javase/19/docs/api/java.base/java/lang/Object.html#notify())|
|atomic|volatile 变量、[java.util.concurrent.atomic](https://docs.oracle.com/en/java/javase/19/docs/api/java.base/java/util/concurrent/atomic/package-summary.html)|
|future|[java.util.concurrent.Future](https://docs.oracle.com/en/java/javase/19/docs/api/java.base/java/util/concurrent/Future.html)|
|interruption|[java.lang.Thread.interrupt](https://docs.oracle.com/en/java/javase/19/docs/api/java.base/java/lang/Thread.html#interrupt())|
|线程安全的容器|[java.util.concurrent](https://docs.oracle.com/en/java/javase/19/docs/api/java.base/java/util/concurrent/package-summary.html) 中的容器|
|线程池|[java.util.concurrent.ThreadPoolExecutor](https://docs.oracle.com/en/java/javase/19/docs/api/java.base/java/util/concurrent/ThreadPoolExecutor.html)|
