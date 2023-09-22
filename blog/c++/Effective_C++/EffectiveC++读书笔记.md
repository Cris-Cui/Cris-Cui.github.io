# Effective C++ 

### 条款1： 视C++为一个语言联邦

**由4种次语言构成:**

- C Part of C++			   [语句statement,  预处理器, 内置数据类型, 指针pointers...]
- Object-Oriented C++ [classes, 封装, 继承, 多态, virtual]
- Template C++
- STL

**对应内置（C-like）类型而言pass-by-value通常比pass-by-reference高效；**

**从C Part of C++ 移往面向对象C++，由于ctor和dtor的存在，pass-by-reference-to**-const**往往更好，Template C++亦是如此**

**STL的迭代器和函数对象都是在C指针之上塑造的， 所以对STL的迭代器和函数对象而言，旧式的C pass-by-value守则再次适用**

### 条款2：

## 资源管理

### 条款16:  成对使用new和delete时要采取相同形式

#### 明白new和delete到底发生了什么

new

1. 调用operator new函数分配内存
2. 针对此内存会有一个或更多构造函数被调用

delete

1. 针对此内存会有一个或更多析构函数被调用
2. 调用operator delete函数释放内存

当对一个指针使用delete，唯一能够让delete知道内存是否存在一个数组大小记录的方法是：**由你来告诉他** ，如果在使用delete的时候加上中括号`delete[]`, delete便认定指针指向一个数组，否则便认定指向的是单一对象.

![image-20230916170243799](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230916170243799.png)

当类型为int,float等内置类型时，new、delete、new[]、delete[]不需要配对使用，当是自定义类型时，new、delete和new[]、delete[]才需要配对使用，当然我们平时编程过程中，为了代码可读性以及为了养成编程良好习惯最好确保所有情况都配对使用。

## 定制new和delete

### 条款





京东

![image-20230916101658954](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230916101658954.png)![image-20230916101713684](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230916101713684.png)

![image-20230916101741816](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230916101741816.png)

![image-20230916101755573](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230916101755573.png)

![image-20230916101808252](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230916101808252.png)

![image-20230916101818735](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230916101818735.png)

![image-20230916101829469](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230916101829469.png)

![image-20230916101839600](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230916101839600.png)

![image-20230916101850226](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230916101850226.png)

![image-20230916101902001](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230916101902001.png)