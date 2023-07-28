# C++11 特性

## 指针

### nullptr

**考虑以下这个例子:**

```cpp
#include <bits/stdc++.h>
using namespace std;
 
// function with integer argument
void fun(int N)   { cout << "fun(int)"; return;}
 
// Overloaded function with char pointer argument
void fun(char* s)  { cout << "fun(char *)"; return;}
 
int main()
{
    // Ideally, it should have called fun(char *),
    // but it causes compiler error.
    fun(NULL); 
}
```

***输出报错***:

```cpp
16:13：错误：重载的“fun(NULL)”调用不明确
     乐趣（空）；
```

C++允许将 NULL 转换为整型. 因此函数调用 fun(NULL) 变得不明确。 

#### GCC的NULL

在C语言中，NULL通常被定义为 :

```cpp
#define NULL ((void *)0)
```

将`NULL`赋值给`int`在C编译器中是可以隐式类型转换成相应的类型, 但是在C++编译器中是要出错的，因为C++是强类型的，`void*`不能隐式转换成其他类型.&#x20;

![](https://kityminder-img.gz.bcebos.com/fb3a66725538d74a9e79f39109be8fa1929ac841)

#### G++的NULL

所以在C+++中，NULL是这样定义的:

```cpp
#ifdef __cplusplus ---简称：cpp c++ 文件
#define NULL 0
#else
#define NULL ((void *)0)
#endif
```

#### C++的 0

> C++中引入0来表示空指针（注：0表示，还是有缺陷不完美）

这样就能明白开始的例子为什么会报错了。
在C++中, NULL就表示0, 我们想要调用期望函数`void fun(char* s);`
可以这样做:

```cpp
fun(static_cast<char*>(0));
fun(static_cast<char*>(NULL));
```

**在C++中，使用0来做为空指针会比使用NULL来做空指针会让你更加警觉。**

#### C++ 11 nullptr

虽然使用0代替NULL表示空指针会让我们更加有警觉性, 但是并没有避免这个问题. 所以, C++ 11 nullptr的出现很好的解决了这个问题.

nullptr关键字用于标识空指针，是std::nullptr\_t类型的（constexpr）变量。它可以转换成任何指针类型和bool布尔类型（主要是为了兼容普通指针可以作为条件判断语句的写法），但是不能被转换为整数。

```cpp
char *p1 = nullptr;     // 正确
int  *p2 = nullptr;     // 正确
bool b = nullptr;       // 正确. if(b)判断为false
int a = nullptr;        // error
```

### 智能指针

> 标头: `#include <memory>`

在实际的C++开发中，我们经常会碰到程序突然崩溃、所使用内存越来越大，最终不得不重启等问题，这些问题往往是由于内存资源管理不当造成的。
比如：

*   有些内存资源已经释放，但指向它的指针没有改变指向(成为了野指针)，并且后续还在使用。
    **出现野指针的常见情况：**

    *   使用未初始化的指针

    ```cpp
    #include <iostream>
    using namespace std;
    int main() {
        int* p;
        cout << *p << endl; //编译通过，运行时出错
    }
    ```

    *   指针所指对象已经消亡(生命周期结束)

    ```cpp
    #include <iostream>
    using namespace std;

    int* retAddr() {
        int num = 10;
        return &num;
    }

    int main() {
        int* p = NULL;
        p = retAddr();
        cout << &p << endl;
        cout << *p << endl; // runtime error：load of null pointer of type 'int'
    }
    ```

    *   指针释放后之后未置空
    *   有些内存资源已经释放，后续还在尝试释放(重复释同一块内存会导致程序运行崩溃)。
    *   没有即使释放不再使用的内存资源，造成内存泄漏 ，程序占用内存资源越来越多。

**什么是智能指针？**
所谓智能指针，从字面意思上来看就是“智能的”指针。其使用方法和普通指针相似，但其在适当时机可以自动的释放分配的内存。使用智能指针可以很好的避免“因为忘记释放而导致的内存泄露”的问题。

**原理其实就是RAII**，在构造的时候获取资源，在析构的时候释放资源，并进行相关指针操作的重载，使用起来就像普通的指针。

#### auto\_ptr(C++17中移除)

##### auto\_ptr存在的问题:

**1. 赋值，参数传递的时候会转移所有权**

```cpp
auto_ptr<A> ap1(new A);
auto_ptr<A> ap2 = nullptr;
ap2 = ap1;
```

ap1和ap3不是指向同一片内存地址， ap1悬空
**这会导致的问题:**

*   `auto_ptr`不能共享所有权, 即两个`auto_ptr`无法指向同一个对象.
*   `auto_ptr`不能作为容器对象，STL容器中的元素经常要支持拷贝，赋值等操作，在这过程中`auto_ptr`会传递所有权

**2. auto\_ptr不能指向数组**
因为auto\_ptr在析构的时候只是调用delete,而数组应该要调用delete\[]。

#### unique\_ptr

> 官方解释: 拥有独有对象所有权语义的智能指针
> 鲁迅说: “它是我的所有物，你们都不能碰它！”

**unique\_ptr的出现了解决了auto\_ptr这样随随便便的所有权转移的问题, 复制构造函数和赋值运算符都在unique\_ptr <>类中被删除。** 这个语法能强调你是在转移所有权，让你清晰的知道自己在做什么，从而不乱调用原有指针。

&#x20;![](https://kityminder-img.gz.bcebos.com/e8a18cc9ea50db3862d4418d082cc71b57e506d2)

通过指针占有并管理另一对象，并在 unique\_ptr 离开作用域时释放该对象的智能指针
在下列两者之一发生时用关联的删除器释放对象：

*   销毁了管理的 unique\_ptr 对象
*   通过 operator=(只接受右值) 或 reset() 赋值另一指针给管理的 unique\_ptr 对象。 ![](https://kityminder-img.gz.bcebos.com/44de2039151d15917a416f4da881d79e75418a6a)

**成员函数**

*   修改器：
    release 返回一个指向被管理对象的指针, 并释放所有权
    reset 替换被管理对象
    swap 交换被管理对象

*   观察器:
    get 返回指向被管理对象的指针

*   使用权传递

```cpp
unique_ptr<AAA> up1(new AAAA);
unique_ptr<AAA> up3;
up3 = std::move(up1);   // 避免重复资源和悬空指针的问题
```

#### shared\_ptr

> 官方解释: 拥有共享对象所有权语义的智能指针
> 鲁迅说: “它是我们(shared\_ptr)的，也是你们(weak\_ptr)的，但实质还是我们的”

每个shared\_ptr对象内部指向两块内存空间:

*   指向对象
*   指向用于引用计数的控制数据

**底层采用引用计数的方式实现的。简单的理解，智能指针在申请堆内存时，会为其配备一个整型值（初始值为1），每当有新对象使用该堆内存时，该整型值+1; 反之，每当使用此堆内存的对象释放时，该整型值-1。当堆空间对应的整型值为0时，即表明不再有对象使用它，该堆空间就会被释放掉。**

##### 三种创建的区别

**1. 构造函数使用原始指针作为参数**

```cpp
std::shared_ptr<int> p1(new int());
```

它在堆上分配两块内存：

*   为int分配的内存
*   用于引用计数的内存，将用于管理与此内存相关的shared\_ptr对象的技术，初始值为1。
    ![](https://kityminder-img.gz.bcebos.com/de9f2e656cfcde465d08d47bbbfe9746ea06cc7f)

**2. std::make\_shared&lt;T&gt;**
为引用计数所需的对象和数据结构做了一次内存分配，即`new`运算符只会被调用一次。
![](https://kityminder-img.gz.bcebos.com/38782773d1198e38ed165661c3bba57d178214a0)
**3. reset()方法**

> 无参reset(): 将引用计数减1; 如果引用计数变为0，则删除指针
> 有参reset(): 在内部指向新的`data`指针，因此其引用计数将再次变为1

##### shared\_ptr注意事项

*   避免循环引用

```cpp
#include <iostream>
#include <memory>

using namespace std;

typedef shared_ptr<int> ptr;
class C;
class B {
   public:
    B() { cout << "B" << endl; }
    ~B() { cout << "~B" << endl; }
    shared_ptr<C> spc;
};

class C {
   public:
    C() { cout << "C" << endl; }
    ~C() { cout << "~C" << endl; }
    shared_ptr<B> spb;
};
int main() {
    {
        shared_ptr<B> pb(new B);
        shared_ptr<C> pc(new C);
        pb->spc = pc;
        pc->spb = pb;
    }
    return 0;
}
```

*   多线程环境注意线程安全

#### weak\_ptr

> 官方解释:到 std::shared\_ptr 所管理对象的弱引用
> 鲁迅说: “它是我们(weak\_ptr)的，也是你们(shared\_ptr)的，但实质还是你们的”

**使用weak\_ptr 指向同一空间, 引用计数不会加1. 是为了辅助shared\_ptr的存在，它只提供了对管理对象的一个访问手段，同时也可以实时动态地知道指向的对象是否存活. 只有某个对象的访问权，而没有它的生命控制权 即是 弱引用，所以weak\_ptr是一种弱引用型指针**

##### weak\_ptr注意事项

*   避免悬空指针
*   使用 weak\_ptr 之前，必须确保其所指向的对象仍然存在
*   std::weak\_ptr 应该通过 shared\_ptr 构造而来，不可直接从裸露指针构造
*   注意失效 当最后一个shared\_ptr 所引用的对象被释放时，weak\_ptr 会自动失效
*   多线程环境注意线程安全

#### 三种智能指针的区别

shared\_ptr 和 unique\_ptr, weak\_ptr 不同点在于：
多个shared\_ptr智能指针可以同时指向同一块堆内存空间, 由于其在实现上采用引用计数的机制，即便有一个shared\_ptr智能指针放弃了对堆内存的“使用权”(引用计数-1), 也不会影响其他指向同一块堆内存的shared\_ptr智能指针（只有引用计数为 0 时，堆内存才会被自动释放）。

#### 使用场景

1、不要使用std::auto\_ptr（已经在C++11或以上标准中弃用）

2、当你需要一个独占资源所有权（访问权+生命控制权）的指针，且不允许任何外界访问，使用std::unique\_ptr

3、当你需要一个共享资源所有权（访问权+生命控制权）的指针，使用std::shared\_ptr

4、当你需要一个能访问资源，但不控制其生命周期的指针，使用std::weak\_ptr

推荐用法：
一个shared\_ptr和n个weak\_ptr搭配使用而不是n个shared\_ptr。
因为一般模型中，最好总是被一个指针控制生命周期，然后可以被n个指针控制访问。

逻辑上，大部分模型的生命在直观上总是受某一样东西直接控制而不是多样东西共同控制。
程序上，能够完全避免生命周期互相控制引发的 循环引用问题。

参考博客: [C++11 智能指针的深度理解 ](http://www.cnblogs.com/KillerAery/)
