# 操作系统、

![image-20230812181105630](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230812181105630.png)

![image-20230812184147367](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230812184147367.png)

![image-20230812185708299](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230812185708299.png)

![image-20230812192538016](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230812192538016.png)

![image-20230812203319765](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230812203319765.png)

![image-20230812204513569](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230812204513569.png)

![image-20230814182659558](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230814182659558.png)

![image-20230814183039249](C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230814183039249.png)

## 进程管理

### 进程

#### 进程五状态变迁&七状态变迁

![](https://cdn.xiaolincoding.com/gh/xiaolincoder/ImageHost/%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F/%E8%BF%9B%E7%A8%8B%E5%92%8C%E7%BA%BF%E7%A8%8B/8-%E8%BF%9B%E7%A8%8B%E4%BA%94%E4%B8%AA%E7%8A%B6%E6%80%81.jpg)



**在虚拟内存管理的操作系统中**,通常将阻塞状态的进程从内存中换出到磁盘，这就是**挂起状态,**没有占用实际的内存空间。

![七种状态变迁](https://cdn.xiaolincoding.com/gh/xiaolincoder/ImageHost/%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F/%E8%BF%9B%E7%A8%8B%E5%92%8C%E7%BA%BF%E7%A8%8B/10-%E8%BF%9B%E7%A8%8B%E4%B8%83%E4%B8%AD%E7%8A%B6%E6%80%81.jpg)

#### 进程PCB

PCB包含:

**进程描述信息**

- 进程标识符
- 用户标识符

**进程控制信息**

- 进程当前状态[new, ready, running, blocked, waiting]
- 进程优先级

**资源分配信息**

- 虚拟内存空间的信息
- 打开的文件描述符或I/O设备

**CPU相关信息**

- CPU寄存器的值

PCB是通过**链表**将相同状态的进程PCB组织在一起，形成就绪队列，运行队列，阻塞队列等。

> 还有一种方式是通过索引的方式组织，但进程会频繁创建和销毁等调度导致状态变化，使用链表能够更灵活的插入和删除

#### 进程上下文切换

> 针对

### 线程

#### 线程安全

### 协程





## C语言编译原理

> 预处理 -> 编译 -> 汇编 -> 链接

1. 预处理 生成.i文件

	- 宏定义替换

	- 条件预编译 ex. 防御式声明
	- 特殊符号 ex. 注释符号
	- 保留#pragma指令

2. 编译 生成.a或.s文件(生成中间文件)

3. 