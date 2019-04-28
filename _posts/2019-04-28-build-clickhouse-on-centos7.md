# 如何在CentOS7系统上搭建Clickhouse编译环境

## 1.前言

最近一直在学习宇宙第一牛逼的OLAP数据库ClickHouse。

它具有的超高的数据压缩比、超快的数据查询速度特别适合时序数据的永久保存和后期分析。

随着学习进度的推进，自己特别想搭建一套ClickHouse的编译环境用于验证自己的一些想法。

本文档主要说明如何在CentOS7系统上搭建一套ClickHouse的编译环境。

## 2.环境准备

本部分主要说明搭建过程用到的软硬件环境

### 2.1.硬件环境

| 序号 | 硬件名称 | 硬件规格      |                                                                                         说明 |
| :--- | -------- | ------------- | -------------------------------------------------------------------------------------------: |
| 1    | CPU      | 4核+          |                                   CPU已经要大于4核，编译的时候可以选择多线程编译；支持SSE4_2 |
| 2    | 内存     | 8GB+,16GB更佳 |                           如果你的编译线程为4，则8GB内存差不多够用；否则推荐用16GB及以上内存 |
| 3    | 硬盘     | 50GB+         | 编译过程会产生大量的.a文件，这些文件会占用大概30GB的磁盘空间，一定要预留出足够的空间用于编译 |


### 2.2.软件环境

| 序号 | 软件名称 | 软件版本 |            说明 |
| :--- | -------- | -------- | --------------: |
| 1    | CentOS   | 7        |            64位 |
| 2    | GCC      | 7+       | 需要7以上的版本 |
| 3    | Cmake    | 3        |                 |


## 3.环境准备

### 3.1.验证处理器是否支持SSE4.2

准备环境前一定要验证处理器是否支持SSE4.2。验证方法如下所示：

```bash
# grep -q sse4_2 /proc/cpuinfo && echo "SSE 4.2 supported" || echo "SSE 4.2 not supported"
```

### 3.2.安装系统编译环境

### 3.2.1.安装依赖软件

由于我采用最小安装的方式安装CentOS7系统，所以必须要先把一些编译依赖的软件包先安装好。

```bash
# yum groupinstall "Development Tools"
# yum install libicu-devel readline-devel mysql-devel openssl-devel unixODBC_devel 
# yum install centos-release-scl-rh epel-release
# yum install --enablerepo='centos-sclo-rh' devtoolset-7-gcc.x86_64 devtoolset-7-gcc-c++.x86_64 cmake3
```


### 3.2.2.开始编译

执行如下命令开始编译

```bash
# git clone --recursive https://github.com/yandex/ClickHouse.git
# cd ClickHouse
# git checkout v19.4.4.33-stable
把仓库checkout到对应的版本
# mkdir build
如果build目录之前已创建需要用rm命令把此目录下的文件都删除
# cd build 
进入到build目录下
# cmake3 .. -DCMAKE_CXX_COMPILER=/opt/rh/devtoolset-7/root/bin/g++ -DCMAKE_C_COMPILER=/opt/rh/devtoolset-7/root/bin/gcc
生成Makefile文件
# make -j4
开始编译
```
## 参考资料

本文档主要参考如下资料：

1. 官方文档
2. https://blog.csdn.net/wusihang9/article/details/77619735

本文档编写目标是为了记录自己的搭建过程。

如果文档内有不合适的地方或者侵犯了原作者某些权益，请及时与我联系，我会尽快修改。