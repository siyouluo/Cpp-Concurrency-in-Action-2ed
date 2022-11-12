# 学习 C++ 多线程编程

C++ 11 标准加入了[并发支持库](https://zh.cppreference.com/w/cpp/thread), 包含线程、原子操作、互斥、条件变量和 future 的内建支持。 
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
