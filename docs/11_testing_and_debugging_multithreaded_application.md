## 并发相关的 bug 类型

* 与并发直接相关的 bug 一般可以分为两大类，一是 unwanted blocking，二是 race condition
* unwanted blocking 包含以下几种情况
  * 死锁（deadlock）：两个线程互相等待，导致均无法完成工作。最明显的情况是，如果负责用户界面的线程死锁，界面将失去响应。也有一些情况是，界面可以保持响应，但一些任务无法完成，比如搜索不返回结果，或者文档不被打印
  * 活锁（livelock）：类似于死锁，不同的是线程不是阻塞等待，而是在忙碌于一个检查循环中，比如自旋锁。严重时，其表现的症状就和死锁一样，比如程序不进行，此外由于线程仍在运行，CPU 会处于高使用率状态。在不太严重的情况下，活锁最终会被操作系统的随机调度解决，但仍然会造成任务的长时间延迟，并且延迟期间 CPU 使用率很高
  * I/O 阻塞或其他外部输入：如果线程阻塞等待外部输入，就无法继续处理工作。因此如果一个线程执行的任务会被其他线程等待，就不要让这个线程等待外部输入
* 许多死锁和活锁都是由于 race condition 造成的，不过很大一部分 race condition 是良性的，比如要处理任务队列的下一个任务，决定用哪个工作线程去处理是无关紧要的。造成问题的 race condtion 包含以下几种情况
  * 数据竞争（data race）：数据竞争是一种特定类型的 race condtion，由于对共享内存位置的不同步的并发访问，它将导致未定义行为。数据竞争通常发生于不正确地使用原子操作来同步线程，或者不加锁访问共享数据
  * 被破坏的不变量（broken invariant）：它可以表现为空悬指针（其他线程可以删除被访问的数据）、随机内存损坏（由于局部更新导致线程读取的值不一致）、双重释放（比如两个线程弹出队列的同一个数据）等。不变量的破坏是暂时的，因为它是基于值的。如果不同线程上的操作要求以一个特定顺序执行，不正确的同步就会导致 race condition，有时就会违反这个执行顺序
  * 生命周期问题（lifetime issue）：这个问题可以归入 broken invariant，但这里单独提出来。这个问题表现为，线程比其访问的数据活得更长。一般这个问题发生于线程引用了超出范围的局部变量，但也不仅限于此，比如调用 [join](https://en.cppreference.com/w/cpp/thread/thread/join)，要考虑异常抛出时，调用不被跳过
* 通常可以通过调试器来确认死锁和活锁的线程以及它们争用的同步对象。对于数据竞争、不变量的破坏、生命周期问题，可见症状（如随机崩溃或不正确的输出）可以显示在代码的任何位置，代码可能重写系统其他部分使用的内存，并且很久以后才被触及，这个错误可能在程序执行的后期出现在与 bug 代码完全无关的位置。这就是共享内存的真正祸端，无论如何限制线程对数据的访问和确保正确的同步，任何线程都可以重写其他线程中的数据

## 定位 bug 的方法

### code review

* 让其他人来审阅代码，因为其他人对代码不熟悉，需要思考代码的工作方式，看待的角度也不一样，更有可能发现潜在的问题。也可以过一段时间自己再看这些代码，能起到相同的效果。审阅多线程代码时，一般要思考以下问题
  * Which data needs to be protected from concurrent access?
  * How do you ensure that the data is protected?
  * Where in the code could other threads be at this time?
  * Which mutexes does this thread hold?
  * Which mutexes might other threads hold?
  * Are there any ordering requirements between the operations done in this thread and those done in another? How are those requirements enforced?
  * Is the data loaded by this thread still valid? Could it have been modified by other threads?
  * If you assume that another thread could be modifying the data, what would that mean and how could you ensure that this never happens?

### 测试

* 测试多线程程序的困难在于，具体的线程调度顺序是不确定的，对于相同的输入，得到的结果却不一定相同，结果可能有时是正确的，有时是错误的。因此存在潜在的 race condition 也不意味着总会得到失败的结果，有时可能也会成功
* 由于重现并发相关的 bug 很困难，所以值得仔细设计测试。最好让每个测试运行最小数量的代码，这样在测试失败时可以最好地隔离出错误代码。比如测试一个并发队列，分别测试并发的 push 和 pop 的工作，就直接比测试整个队列的功能要好
* 为了验证问题是否与并发相关，应该从测试中消除并发性。多线程中的 bug 并不意味着一定是并发相关的，如果一个问题在单线程中也总是出现，这就是一个普通的 bug，而不是并发相关的 bug。如果一个问题在单核系统中消失，而在多核或多处理器系统中总会出现，一般这就可能是一个 race condition，或同步、内存序相关的问题
* 如果要测试并发队列，需要考虑以下其情况
  * One thread calling  push() or  pop() on its own to verify that the queue works at a basic level
  * One thread calling  push() on an empty queue while another thread calls pop()
  * Multiple threads calling  push() on an empty queue
  * Multiple threads calling  push() on a full queue
  * Multiple threads calling  pop() on an empty queue
  * Multiple threads calling  pop() on a full queue
  * Multiple threads calling  pop() on a partially full queue with insufficient items for all threads
  * Multiple threads calling  push() while one thread calls  pop() on an empty queue
  * Multiple threads calling  push() while one thread calls  pop() on a full queue
  * Multiple threads calling  push() while multiple threads call  pop() on an empty queue
  * Multiple threads calling  push() while multiple threads call  pop() on a full queue
* 考虑完以上情况之外，接着就要考虑测试环境的因素
  * What you mean by “multiple threads” in each case (3, 4, 1,024?)
  * Whether there are enough processing cores in the system for each thread to run on its own core
  * Which processor architectures the tests should be run on
  * How you ensure suitable scheduling for the “while” parts of your tests
* 一般满足以下条件的代码就是易于测试的，这些条件单线程和多线程中同样适用
  * 每个函数和类的责任是清晰的
  * 函数简明扼要（short and to the point）
  * 测试可以完全控制被测试代码的所在环境
  * 执行特定操作的被测试代码在系统中是紧密而非分散的
  * 代码在写下之前已被考虑过如何测试
* 为了测试设计并发代码的一个最好方法是消除并发，如果可以把代码分解成负责线程间通信路径的部分，以及在单线程中操作通信数据的部分，就可以极大地简化问题。对于操作通信数据的部分就可以用常规的单线程技术测试，对于负责线程间通信的部分，代码小了很多，测试也更容易

### 多线程测试技术

* 第一种测试技术是蛮力测试（也叫压力测试），随着代码运行次数的增加，bug出现的几率也更高，如果代码运行十亿次都通过，代码就很可能是没有问题的。如果测试是细粒度的（fine-grained），比如前面对并发队列的测试，蛮力测试就更可靠。如果粒度非常大，可能的组合也非常多，即使十亿次的测试的结果也不算可靠
* 蛮力测试的缺点是，如果测试本来就保证了问题不会发生，那么无论测试多少次都不会出现失败的情况，这就会造成误导。比如在单核系统上测试多线程程序，race condition 和乒乓缓存的问题根本不会出现，但这不表示这个程序在多核系统上是没问题的。又比如，不同处理器架构提供了不同的同步和内存序工具，在 x86 和 x86-64 架构上，无论使用 [memory_order_relaxed](https://en.cppreference.com/w/cpp/atomic/memory_order) 还是 [memory_order_seq_cst](https://en.cppreference.com/w/cpp/atomic/memory_order) 内存序，原子 load 操作总是一样的，这意味着在 x86 架构上使用 relaxed 语义总是可行的，但如果换成细粒度内存序指令的系统（比如 SPARC）就会失败
* 第二种测试技术是组合仿真测试（combination simulation testing），即使用一个特殊的软件来仿真真实的运行时环境。仿真软件将记录数据访问、锁定、原子操作的序列，然后使用 C++ 内存模型的规则来重复运行所有可能的操作组合，以确定 race condition 和死锁
* 虽然这种详尽的组合测试可以保证找到设计所要检测的所有问题，但会花费大量时间，因为组合的数量随线程 数和每个线程执行的操作数呈指数增长，它最好用于单个代码片段的细粒度测试，而非用于整个程序。这种技术的另一个明显缺点是，它要求访真软件能处理代码中的操作
* 第三种测试技术是使用专门的库。比如共享数据通常会用 mutex 保护，如果在访问数据时能检查哪些 mutex 被锁定了，就能验证线程在访问数据时是否锁定了相应的 mutex，如果没有锁定就报告失败。库实现也能记录上锁的顺序，如果另一个线程对同一个 mutex 以不同顺序上锁，这就会被记录为潜在的死锁
* 另一种类型的库是，同步原语的实现允许测试编写者在多线程等待时，可以控制哪个线程来获得锁，或者哪个线程被 [notify_one](https://en.cppreference.com/w/cpp/thread/condition_variable/notify_one) 通知。这就允许设置特定方案，来验证代码是否在这些方案中按预期运行
* 一些测试工具已经作为标准库实现的一部分提供了，其他的则可以基于标准库的部分手动实现

### 构建多线程测试代码

* 多线程测试代码可以分为以下几部分
  * 必须先执行的总体设置
  * 必须运行在每个线程上的线程特定的设置
  * 要并发运行在每个线程上的代码
  * 并发执行结束后的状态断言
* 如下是对一个队列的测试代码

```cpp
void test_concurrent_push_and_pop_on_empty_queue() {
  ConcurrentQueue<int> q;  // 总体设置：先创建一个队列
  std::promise<void> go, push_ready, pop_ready;
  std::shared_future<void> ready(go.get_future());
  std::future<void> push_done;
  std::future<int> pop_done;
  try {
    push_done = std::async(
        std::launch::async,  // 指定异步策略保证每个任务运行在自己的线程上
        [&q, ready, &push_ready]() {
          push_ready.set_value();
          ready.wait();
          q.push(42);  // 线程特定的设置：存入一个 int
        });
    pop_done = std::async(std::launch::async, [&q, ready, &pop_ready]() {
      pop_ready.set_value();
      ready.wait();
      return q.try_pop();
    });
    push_ready.get_future().wait();  // 等待开始测试的通知
    pop_ready.get_future().wait();   // 同上
    go.set_value();                  // 通知开始真正的测试
    push_done.get();                 // 获取结果
    assert(pop_done.get() == 42);    // 获取结果
    assert(q.empty());
  } catch (...) {
    go.set_value();  // 避免空悬指针
    throw;           // 再抛出异常
  }
}
```

### 测试多线程代码的性能

* 使用并发的一个主要目的就是利用多核处理器来提高程序性能，因此测试代码来确保性能确实提升了是很重要的。性能相关的一个主要方面就是可扩展性，性能应该随着核数一起提升。在测试多线程代码性能时，最好在尽可能多的不同配置上进行测试
