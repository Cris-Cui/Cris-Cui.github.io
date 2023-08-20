# 安装软件包

## 安装Nginx



## 安装Yaml-Cpp

```shell
从GitHub上下载源码编译安装：git clone https://github.com/jbeder/yaml-cpp.git；
进入源码目录并创建一个 build 目录：cd yaml-cpp && mkdir build && cd build；
cmake 一下：cmake -DYAML_BUILD_SHARED_LIBS=on ..，选项表示生成共享库，..表示 cmake 所需的 CMakeList.txt 在上一级目录中；
常规操作 sudo make然后sudo make install；
需要sudo ldconfig更新一下共享库；
头文件在/usr/local/include，库文件在/usr/local/lib。
```

## 安装Boost库

```shell
sudo apt-get install libboost-all-dev
```



# Linux常用命令

## 给普通用户加权

>  编辑/etc/sudoers文件，找到这一 行："**root ALL=(ALL) ALL**"在起下面添加"**xxx ALL=(ALL) ALL**"(这里的xxx是你的用户名)，然后保存。

## netstat

#### **netstat -tunlp**

> 用于显示 tcp，udp 的端口和进程等相关情况。

## 换源

