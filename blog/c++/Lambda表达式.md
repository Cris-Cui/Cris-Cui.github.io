# Lambdas

> C++ 11 引入 Lambdas, 允许定义**inline function**。能够被用来当作**参数**或者是**Local Object(函数对象)**

## 基本语法

```cpp
/**
 * @param [...] 捕获以在lambda内部访问的非静态外部对象。可以使用std::cout这样的静态对象。
 * @details 
 * 	  	[=] passed by value (只读访问, 通过mutable打破限制)
 * 		[&] passed by refence
 * @param (...) optional可选的, 
 * @param mutable 在按值传递的外部对象具有写权限
 */
[...](...)mutable throwSpec -> retType {...}
```

**Lambdas函数对象的类型是一个匿名函数对象(or 仿函数)**

