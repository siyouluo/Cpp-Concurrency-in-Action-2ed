## 模板
> 模板类型参数前必须使用关键字 `typename` 或 `class`, 这两者是等价的, 含义相同，可以互换使用，甚至在一个模板中可以混合使用。  
但是用关键字 typename 来指定模板类型参数比 class 更加直观。因为模板类型实参可以是任意类型，而不一定是“类”。 而且 typename 更加清除地指出随后的名字是一个 类型名。
只是由于 typename 是在模板已经广泛使用之后才引入 C++ 语言的，某些程序员仍然只用 class。
> —— C++ Primer 第5版 16.1.1节 函数模板
```cpp
template <typename T, class U>
void foo(const T&, const U&);
```

## 可变参数模板

> 一个 **可变参数模板 （variadic template）** 就是一个接受可变数目参数的**模板函数**或**模板类**。可变数目的参数被称为 **参数包 (parameter packet)**。
> 存在两种参数包：
> **模板参数包(template parameter packet)** ，表示零个或多个模板参数；
> **函数参数包(function parameter packet)** ，表示零个或多个函数参数。
> —— C++ Primer 第5版 16.4节


1. 可变参数
C++中通过省略号来表明参数数目是可变的，即该参数实际上是一个参数包，例如 
`typename... Args` 和 `class... Args` 中 `Args` 就是一个模板参数包。
通过 `sizeof...()` 可以知道模板参数包中实例化后到底有多少个参数。

2. 可变参数模板函数

```cpp
#include <iostream>
#include <string>
// Args 是一个模板参数包; rest 是一个函数参数包
// Args 表示零个或多个模板类型参数
// rest 表示零个或多个函数参数
template <typename T, typename ... Args>
void foo(const T &t, const Args& ... rest)
{
    std::cout << "sizeof...(Args) = " << sizeof...(Args) << std::endl;
    std::cout << "sizeof...(rest) = " << sizeof...(rest) << std::endl;
}

int main()
{
    int i=0;
    double d = 3.14;
    std::string s = "how now brown cow";
    foo(i,s,42,d);
    foo(s,42,"hi");
    foo(d,s);
    foo("hi");
    return 0;
}

```

```cmake CMakeLists.txt
cmake_minimum_required(VERSION 3.18)

project(demo)

add_executable(demo demo.cpp)
```


```plain stdout
sizeof...(Args) = 3
sizeof...(rest) = 3
sizeof...(Args) = 2
sizeof...(rest) = 2
sizeof...(Args) = 1
sizeof...(rest) = 1
sizeof...(Args) = 0
sizeof...(rest) = 0
```

3. 可变参数函数模板的递归调用   

可变参数函数通常是递归的，第一次调用处理包中的第一个实参，然后用剩余实参调用自身。但是需要手动定义一个递归基，来处理参数包中的最后一个参数，以终止递归调用。
```cpp
#include <iostream>
#include <string>
// 递归基: 用来终止递归并打印最后一个元素的函数
// 此函数必须在可变参数版本的 print 定义之前声明，因为会被它调用
// 最后一次调用为 print(std::cout, d);
template <typename T>
std::ostream &print(std::ostream &os, const T &t)
{
    return os << t; // 包中最后一个元素之后不打印分隔符
}

// 包中除了最后一个元素之外的其他元素都会调用这个版本的print
template <typename T, typename... Args>
std::ostream &print(std::ostream &os, const T &t, const Args& ... rest)
{
    os << t << ", "; // 打印第一个实参
    return print(os, rest...); // 递归调用, 打印其他实参
}

int main()
{
    int i=0;
    double d = 3.14;
    std::string s = "how now brown cow";
    print(std::cout, i, s, 42, d);
    return 0;
}

```

```plain stdout
0, how now brown cow, 42, 3.14
```



## 右值引用
- [C++11右值引用（一看即懂） | C语言中文网](http://c.biancheng.net/view/7829.html)
右值引用主要用于移动语义和完美转发
1. 右值引用
   ```cpp
   int && a = 10; // 对右值进行引用
   a = 100; // 同时保留对右值进行修改的操作
   cout << a << endl; // 输出 100
   ```
2. 移动语义
   - [C++移动语义 详细讲解【Cherno C++教程】](https://www.cnblogs.com/zhangyi1357/p/16018810.html)
   创建右值用作某个对象的构造，直接将该右值变量移动给该对象的成员变量，而无需重新申请内存空间。
   ```cpp
   //移动构造函数
   String(String&& other) {
     printf("Moved!\n");
     m_Size = other.m_Size;
     m_Data = other.m_Data;
     other.m_Data = nullptr;
     other.m_Size = 0;
   }



   Entity(String&& name) // 接收右值引用的函数，在参数传进来后其右值属性就退化了，所以给m_Name的参数仍然是左值，还是会调用复制构造函数，而非移动构造函数
     : m_Name(name) {}

   Entity(String&& name)
       :m_Name((String&&)name) {} // 强制类型转换为右值引用，将调用移动构造函数

   Entity(String&& name)
       :m_Name(std::move(name)) {} // 通过 std::move 将左值转换为右值, 调用移动构造函数
   ```
3. 完美转发
   - [C++11完美转发及实现方法详解](http://c.biancheng.net/view/7868.html)
   在上面的例子中发现，`Entity(String&& name)` 即便传入的实参是右值，在参数传入之后右值属性就退化了，变成了左值，交给 m_Name 进行拷贝构造。这就不是完美转发，因为变量的左右属性发生了变化。

    再讲一遍：
   ```cpp
    template<typename T>
    void function(T t) {
        otherdef(t);
    }
   ```
   如上所示，function() 函数模板中调用了 otherdef() 函数。在此基础上，完美转发指的是：如果 function() 函数接收到的参数 t 为左值，那么该函数传递给 otherdef() 的参数 t 也是左值；反之如果 function() 函数接收到的参数 t 为右值，那么传递给 otherdef() 函数的参数 t 也必须为右值。

   显然，function() 函数模板并没有实现完美转发。一方面，参数 t 为非引用类型，这意味着在调用 function() 函数时，实参将值传递给形参的过程就需要额外进行一次拷贝操作；另一方面，无论调用 function() 函数模板时传递给参数 t 的是左值还是右值，对于函数内部的参数 t 来说，它有自己的名称，也可以获取它的存储地址，因此它永远都是左值，也就是说，传递给 otherdef() 函数的参数 t 永远都是左值。总之，无论从那个角度看，function() 函数的定义都不“完美”。 

   ```cpp
   //实现完美转发的函数模板
   // 传入 function 的实参所具有的左值、右值属性都能够完美地传递到 otherdef 函数内部
   template <typename T>
   void function(T&& t) {
     otherdef(forward<T>(t));
   }
   ```

## bind

- [std::bind | C++中文参考手册](https://zh.cppreference.com/w/cpp/utility/functional/bind)
```cpp
void f(int n1, int n2, int n3, const int& n4, int n5)
{
    std::cout << n1 << ' ' << n2 << ' ' << n3 << ' ' << n4 << ' ' << n5 << '\n';
}
// 演示参数重排序和按引用传递
int n = 7;
// （ _1 与 _2 来自 std::placeholders ，并表示将来会传递给 f1 的参数）
auto f1 = std::bind(f, _2, 42, _1, std::cref(n), n);
n = 10;
f1(1, 2, 1001); // 1 为 _1 所绑定， 2 为 _2 所绑定，不使用 1001
                // 相当于调用 f(2, 42, 1, n, 7) 
```

## lambda 表达式
- [Lambda 表达式 | C++中文参考手册](https://zh.cppreference.com/w/cpp/language/lambda)

## 尾置返回类型
- [C++11 尾置返回类型](https://www.jianshu.com/p/2d44dae53910)
对于返回值比较复杂的函数声明，可以结合 auto 写作如下形式
```cpp
// 首先将返回值设为 auto, 然后通过 -> 来指定返回值类型是一个指向维度为10的数组的指针
auto func(int i) -> int (*)[10]
```
如果不用尾置返回类型，那么就要写成一个极其复杂的形式：
```cpp
int (*func(int i))[10]
```

lambda表达式的尾置返回
```cpp
std::vector<int> v {1, -9, 8, -3, 5};
transform(v.begin(), v.end(), v.begin(), [](int i) -> int {
    if (i<0) return -i; else return i;
});

```

## std::result_of
- https://zh.cppreference.com/w/cpp/types/result_of


