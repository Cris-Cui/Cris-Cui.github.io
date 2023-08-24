# auto & decltype

## auto

> 在C++11中，您可以使用auto声明一个变量或对象，而无需指定其特定类型. 当类型是一个相当长 and/or 复杂的表达式时，使用auto特别有用
>
> ```cpp
> vector<string> vec;
> auto pos = vec.begin();				// 代替长长的迭代器类型
> 
> auto l = [](int x)->bool { ... }	// 代替lambda表达式object
> ```
>
> 