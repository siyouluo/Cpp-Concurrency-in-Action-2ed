# 学习 C++ 多线程编程

C++ 11 标准加入了[并发支持库](https://zh.cppreference.com/w/cpp/thread), 包含线程、原子操作、互斥、条件变量和 future 等。 
此后要用 C++ 写多线程并发的程序, 不再需要调用平台相关的 API, 如 [Windows API](https://learn.microsoft.com/zh-cn/windows/win32/procthread/multiple-threads) 和 [POSIX thread (pthread)](https://www.cs.cmu.edu/afs/cs/academic/class/15492-f07/www/pthreads.html).

通过阅读 [1] 及其中文翻译版 [2] 来学习 C++ 多线程编程，相关笔记放在 [docs](docs/) 文件夹内, 
可通过[目录](docs/index.md) 查看.


参考资料  
- [1] [Anthony Williams. C++ Cocurrency In Action[M]，Second Edition](https://www.cplusplusconcurrencyinaction.com/)
- [2] [C++并发编程实战 第2版 王高飞译](books/C++并发编程实战第2版-王高飞译.pdf)
- [3] https://github.com/progschj/ThreadPool
- [4] [c++ 11 线程池---完全使用c++ 11新特性](https://www.cnblogs.com/microDeLe/p/16010882.html)
- [5] https://github.com/lzpong/threadpool
- [6] [C++ std::thread - C++ 11 标准库 | 菜鸟教程](https://www.runoob.com/w3cnote/cpp-std-thread.html)
- [C++ 多线程 - Windows | 菜鸟教程](https://www.runoob.com/w3cnote/cpp-multithread-demo.html)
- [C++ 多线程 -  POSIX | 菜鸟教程](https://www.runoob.com/cplusplus/cpp-multithreading.html)
- [C++ Cocurrency In Action 第二版配套源码 anthonywilliams/ccia_code_samples | Github](https://github.com/anthonywilliams/ccia_code_samples)
- [《C++ 并发编程指南》傅海平 | Github](https://github.com/forhappy/Cplusplus-Concurrency-In-Practice)
- [C++并发编程实战 第2版 陈晓伟译 | Github](https://github.com/xiaoweiChen/CPP-Concurrency-In-Action-2ed-2019)





C++11 引入了 Boost 线程库作为标准线程库，作者 Anthony Williams 为介绍其特性，于 2012 年出版了 [C++ Concurrency in Action](https://book.douban.com/subject/4130141/) 一书，并顺应 C++17 于 2019 年 2 月出版了[第二版](https://book.douban.com/subject/27036085/)。

[C++ Concurrency in Action 2ed](https://learning.oreilly.com/library/view/c-concurrency-in/9781617294693/) 前五章介绍了[线程支持库](https://en.cppreference.com/w/cpp/thread)的基本用法，
后六章从实践角度介绍了并发编程的设计思想，相比第一版多介绍了一些 C++17 特性，如 [std::scoped_lock](https://en.cppreference.com/w/cpp/thread/scoped_lock)、[std::shared_mutex](https://en.cppreference.com/w/cpp/thread/shared_mutex)，
并多出一章（第十章）介绍 [C++17 标准库并行算法](https://en.cppreference.com/w/cpp/header/execution)，
此外个人会在相应处补充 C++20 相关特性，如 
- [std::jthread](https://en.cppreference.com/w/cpp/thread/jthread)
- [std::counting_semaphore](https://en.cppreference.com/w/cpp/thread/counting_semaphore)
- [std::barrier](https://en.cppreference.com/w/cpp/thread/barrier)
- [std::latch](https://en.cppreference.com/w/cpp/thread/latch) 等。

阅读本书前可参考 [Andrew S. Tanenbaum](https://en.wikipedia.org/wiki/Andrew_S._Tanenbaum) 的 [*Modern Operating Systems*](https://book.douban.com/subject/25864553/)，预备操作系统的基础知识  
- [进程与线程](reference/processes_and_threads.html)
- [死锁](reference/deadlocks.html)
- [内存管理](reference/memory_management.html)
- [文件系统](reference/file_systems.html)
- [I/O](reference/IO.html)

此为个人笔记，仅供参考，更详细内容见[原书](https://learning.oreilly.com/library/view/c-concurrency-in/9781617294693/)。

















