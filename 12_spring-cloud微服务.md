| 微服务组件               |                                                              |
| ------------------------ | ------------------------------------------------------------ |
| Nacos, Zookeeper, Eureke | 服务注册与发现; 配置中心, 推送配置变更.                      |
| OpenFeign                | 服务之间通讯                                                 |
| Gateway, Zuul            | 外部先访问gateway, gateway从注册中心找到对应服务, 再将请求分发给具体的服务 |
| Sentinel, hystrix        | 某个服务出现问题, 进行服务熔断                               |
| robbon, loadbalance      | 负载聚合器                                                   |
| skywalking               | 链路追踪                                                     |
| Seata                    | 分布式事务. 用户下了订单, 订单要修改订单数据库, 用户需要修改用户库. 多个数据库的事务. |



微服务之间如何独立通讯?

同步通信: Dubbo rpc; SpringCloud rest.

异步通信: MQ.



# Nacos

Nacos1.x是http, 2.x的通信机制是grpc.

grpc是rpc框架, 基于HTTP2, 支持双向流式通信. 数据的序列化采用protocol buffer, 二进制的.

HTTP主要用在浏览器和服务器. 数据的序列化采用Json.



服务启动的时候, 与Nacos简历GRPC连接, 注册服务, 



## 服务

## 配置

# OpenFeign



# Gateway



# Sentinel



# Seata

