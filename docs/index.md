## 并发支持库
1. [C++并发概述](01_hello_world_of_concurrency_in_cpp.md)
2. [线程管理（Managing thread）](02_managing_thread.md)：[\<thread\>](https://en.cppreference.com/w/cpp/header/thread)
3. [线程间共享数据（Sharing data between thread）](03_sharing_data_between_thread.md)：[\<mutex\>](https://en.cppreference.com/w/cpp/header/mutex)、[\<shared_mutex\>](https://en.cppreference.com/w/cpp/header/shared_mutex)
4. [同步并发操作（Synchronizing concurrent operation）](04_synchronizing_concurrent_operation.md)：[\<condition_variable\>](https://en.cppreference.com/w/cpp/header/condition_variable)、[\<semaphore\>](https://en.cppreference.com/w/cpp/header/semaphore)、[\<barrier\>](https://en.cppreference.com/w/cpp/header/barrier)、[\<latch\>](https://en.cppreference.com/w/cpp/header/latch)、[\<future\>](https://en.cppreference.com/w/cpp/header/future)、[\<chrono\>](https://en.cppreference.com/w/cpp/header/chrono)、[\<ratio\>](https://en.cppreference.com/w/cpp/header/ratio)
5. [C++ 内存模型和基于原子类型的操作（The C++ memory model and operations on atomic type）](05_the_cpp_memory_model_and_operations_on_atomic_type.md)：[\<atomic\>](https://en.cppreference.com/w/cpp/header/atomic)

## 并发编程实践

6. [基于锁的并发数据结构的设计（Designing lock-based concurrent data structure）](06_designing_lock_based_concurrent_data_structure.md)
7. [无锁并发数据结构的设计（Designing lock-free concurrent data structure）](07_designing_lock_free_concurrent_data_structure.md)
8. [并发代码的设计（Designing concurrent code）](08_designing_concurrent_code.md)
9. [高级线程管理（Advanced thread management）](09_advanced_thread_management.md)
10. [并行算法（Parallel algorithm）](10_parallel_algorithm.md)：[\<execution\>](https://en.cppreference.com/w/cpp/header/execution)
11. [多线程应用的测试与调试（Testing and debugging multithreaded application）](11_testing_and_debugging_multithreaded_application.md)

## 附录
- [A. C++ 11 语言特性参考](Appx_A_brief_reference_for_some_cpp11_language_features.md)

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
