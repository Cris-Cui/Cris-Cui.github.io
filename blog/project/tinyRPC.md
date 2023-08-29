# TinyRPC学习记录

## 源码下载并运行

```shell
git clone git@github.com:Gooddbird/tinyrpc.git
```

### 依赖检查

#### Google ProtoBuf

```shell
protoc --version
# protobuf-3.19.5 安装
cd protobuf-3.19.5
./autogen.sh # 生成配置文件
./configure  # 配置环境
make		 # 编译源码
sudo make install # 安装
sudo ldconfig # 刷新动态库
```

#### TinyXML

```shell
# 下载地址：https://sourceforge.net/projects/tinyxml/
# 要生成 libtinyxml.a 静态库，需要简单修改 makefile 如下:
# 84 行修改为如下
OUTPUT := libtinyxml.a 

# 194, 105 行修改如下
${OUTPUT}: ${OBJS}
	${AR} $@ ${LDFLAGS} ${OBJS} ${LIBS} ${EXTRA_LIBS}

# 安装过程
cd tinyxml
make -j4

# copy 库文件到系统库文件搜索路径下
cp libtinyxml.a /usr/lib/

# copy 头文件到系统头文件搜索路径下 
mkdir /usr/include/tinyxml
cp *.h /usr/include/tinyxml
```

**因为日志涉及到线程安全的问题，所以在写日志模块之前，先写线程相关的:**

## 线程模块

> 自C++11之后，标准库提出了std::thread, 但是由于它没有分读写锁, 不适用高并发场景(写少读多), 会有比较大的性能损失, 所以选择Linux 下的**pthread**, 对其进行**RAII**的封装.



## 线程安全模块

### 局部锁ScopedLock

> 线程同步利器 —— 区域锁(局部锁)
>
> 将**RAII**手法应用于locking的并发编程技巧。其具体做法就是**在构造时获得锁，在析构时释放锁**.
>
> > RAII思想(**R**esource **A**cquisition **I**s **I**nitialization)
> >
> > 资源的使用一般包括三个环节, a.获取资源 b.使用资源 c.销毁资源
> >
> > 但第三步销毁资源是经常容易忘记的一个环境, 使用RAII思想，充分利用C++局部变量对象自动销毁的特性控制资源的生命周期。
>
> 该区域锁(局部锁)使用该思想设计。

<img src="C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230827131805840.png" alt="image-20230827131805840" style="zoom:50%;" />

### 互斥锁Mutex

<img src="C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230827091917590.png" alt="image-20230827091917590" style="zoom:50%;" />

### 自旋锁SpinLock

<img src="C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230827091939122.png" alt="image-20230827091939122" style="zoom:50%;" />

### 原子锁CASLock

<img src="C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230827092023579.png" alt="image-20230827092023579" style="zoom:50%;" />

### 读写锁RWLock

> 由于读写锁有一把读锁, 一把写锁, 所以常规的局部锁不太适用了, 为读写锁封装特化的局部读锁和局部写锁
>
> <img src="C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230827093143737.png" alt="image-20230827093143737" style="zoom:50%;" />

<img src="C:%5CUsers%5C28568%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20230827102532430.png" alt="image-20230827102532430" style="zoom:50%;" />

### 信号量

## 日志模块

> 对于C++程序而言, 最好整个程序(包括主程序和程序库)都使用相同的日志库, 程序有一个整体的日志输出, 而不要各个组件有各自的日志输出, 从这个意义上来讲, 日志是一个Singleton。

**难点:**将日志数据从多个前端(frontend)高效地传输到后端(backend). 这是一个典型的**多生产者-单消费者问题**。
对于生产者而言, 要尽量做到低延迟, 低CPU开销, 无阻塞; 对于消费者而言, 要做到足够大的吞吐量, 并占用较少资源.



主要有两种API风格:

- C Printf风格
- C++ Stream流式风格

在这里，采用**C++Stream流式风格**使用更自然，不用费心保持Printf的格式字符串与其对应的参数类型的一致性. 且在C++11 variadic template 之前很费劲处理printf(fmt, ...)

### 功能需求

常规通过日志库如Log4j/Log4Cpp, 会提供丰富的功能，但这些功能不是都是必需的。

1. 日志级别, 如ERROR，WARN, INFO, DEBUG等
2. 日志输出地, 如文件, socket, stdout等
3. 日志消息的格式(Layout)

### 性能需求

编写Linux服务端序的时候，我们需要一个高效的日志库。只有日志库足够高效，程序员才敢在代码中输出足够多的诊断信息，减小运维难度，提升效率。高效性体现在几方面:

- 每秒写几千上万条日志的时候没有明显的性能损失
- 能应对一个进程产生大量日志数据的场景，例如1GB/min。
- 不阻塞正常的执行流程。
- 在多线程程序中，不造成争用(contention)。

### 多线程异步日志

> 在设计初期，主要参考了Sylar的日志实现，在开发到一定程度后，发现同步日志或多或少有些影响性能，毕竟每次写入文件的磁盘IO还是比较耗时的。遂改为异步日志。

#### 实现

