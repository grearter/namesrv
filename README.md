# 什么叫名字服务？通过名字服务获取实际服务ip:port，由名字服务提供负载均衡，故障转移等功能。

## 已实现功能如下：
1. 为每个服务注册一个名字，比如：u.srv.ns
2. 一个名字下对应的多台机器按带权重的round-robin方式调度
3. 对服务ip:port进行监控，能做到实时转移故障（对udp端口监听型服务也支持）
4. 无中心节点依赖问题，master可以做到在线无损伸缩扩容
5. 本机agent支持6万qps调用
6. 支持统计功能
7. 目前调度策略，一段时间复用上一个ip:port，这样对有些场景的缓存重用有好处

## 安装方式：
1. 每台机器（包括调用机和被调用机）需安装agent服务，agent服务监听127.0.0.1:8328/udp，优先考虑和cmonitor（通用进程监控服务，毫秒级拉起服务，无资源占用）一起打包安装，可做为后续机器安装配置标配
2. master服务可安装到若干台机器上，需要对agent暴露地址，可使用域名，这里对域名的使用很弱，基本只是agent第一次启动时才会调用一次
3. 调用方通过集成api获取相应服务名字对应的ip:port，目前已提供php/python/c/golang四种api代码供集成，集成方式非常简单

## 名字服务后续支持计划：
1. 给名字服务提供一个比较好用的图形管理界面

## 注：
1. 名字服务包含agent, master两个程序，分别对应namecli, namesrv，namecli需部署到本机，namesrv可部署多个
2. namesrv用到的存储目前是mongo