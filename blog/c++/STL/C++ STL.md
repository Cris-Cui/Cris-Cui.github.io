> 使用一个东西，
>
> 却不明白它的道理，
>
> 不高明!

# C++ 标准库

## 泛型编程 Generic Programming，GP

> 探讨GP与OOP的根本差异，templates 的意义和运用

**STL则是C++泛型编程最成功的例子.**

## STL

> 六大部件:
>
> - 容器(Containers)
>
> 	存放数据的各种数据结构	
>
> - 分配器(Allocators)
>
> 	负责空间的配置，管理和释放，是一个class template
>
> - 算法(Algorithms)
>
> - 迭代器(Iterators)
>
> 	从实现角度来看，迭代器是一种将operator*, operator->, operator++, operator--等指针相关操作重载的class template；原生指针也是一种迭代器
>
> - 适配器(Adapters)
>
> - 仿函数(Functors)
>
> 	行为类型函数，重载operator()的class或class template

"前闭后开"区间



### 容器

#### vector

vector迭代器：由于vector维护的是⼀个线性区间，所以**普通指针**具备作为vector迭代器的所有条件，就不需要重载operator+，operator*之类的东西

### 迭代器

### 分配器

> 默认分配器是`std::alloctor`, 包含两个函数allocate与deallocte，这两个函数分别调⽤operator new()与delete()，这两个函数的底层又分别是malloc()and free();但是每次malloc会带来格外开销（因为每次malloc⼀个元素都要带有附加信息）

### 适配器

### 算法

### 仿函数

