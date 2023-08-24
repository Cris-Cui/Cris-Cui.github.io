## 可变参数模板Variadic Templates

> 是对C++98 泛型编程(类模板, 函数模板)只能包含固定数量的模板参数的进行的一个高度泛化。它能表示从0到任意多个，任意类型的模板类型参数。

### 写法上的区别

> **...**  : 就是所谓的一个pack(包)
>
> 用于模板参数, 就是模板参数包template parameters pack
>
> 用于函数参数类型, 就是函数参数类型包function parameters types pack
>
> 用于函数参数, 就是函数参数包function parameters pack

```cpp
template <typename... T>		// 声明一个参数包T... args，这个参数包中可以包含0到任意个模板参数；
func(T... args) {	
    cout << sizeof...(args) << endl;	// sizeof... 打印变参的个数
}

template <typename... T>
func(const T&... args);
```

## Alias Template(template typedef)

