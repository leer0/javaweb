## Dubbo

#### 一、RPC（Remote Procedure Call）

1、基本原理

![](C:\Users\leer0\Pictures\rpc.PNG)

![](C:\Users\leer0\Pictures\rpc时序图.PNG)

注意：影响RPC框架性能的因素有两点：

（1）框架能否快速的在个服务器之间建立起链接（通信效率）

（2）框架的序列化与反序列化是否快速（序列化与反序列化效率）

#### 二、dubbo架构

Apache Dubbo是一款高性能、轻量级的开源Java RPC框架，它提供三大核心能力：面向接口的远程方法调用，智能容错和负载均衡，以及服务自动注册和发现。

![](C:\Users\leer0\Pictures\DubboArchitecture.PNG)

**1、搭建注册中心（Register）**

dubbo选用zookeeper注册中心

**zookeeper环境搭建步骤：**

（1）下载并解压apache-zookeeper压缩包并解压

（2）复制粘贴一份apache-zookeeper/conf/zoo_sample.cfg并重命名为zoo.cfg

（3）打开zoo.cfg文件，修改dataDir属性为自定义的路径，如dataDir==../data

（4）双击bin目录下的zkServer.cmd文件启动zookeeper

（5）运行bin目录下的zkCli.cmd来连接zookeeper服务器

zookeeper是Apache Hadoop的子项目，是一个树形的目录服务，支持变更推送，适合作为Dubbo服务的注册中心，工业强度较高，可用于生产环境。

![](C:\Users\leer0\Pictures\zookeeper.PNG)

**2、安装监控中心（Monitor）**

[具体详见](https://github.com/apache/dubbo-admin)

#### 三、dubbo-helloworld

1、提出需求

#### 四、zookeeper宕机与dubbo直连

![](C:\Users\leer0\Pictures\zookeeper宕机.PNG)

#### 五、负载均衡机制

1、Random LoadBalance（dubbo默认使用）

基于权重的随机负载均衡机制

2、RoundRobin LoadBalance

基于权重的的轮询负载均衡机制

3、ConsistentHash LoadBalance

一致性哈希负载均衡机制

4、LeastActive LoadBalance

最少活跃调用数均衡机制