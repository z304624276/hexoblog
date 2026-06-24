---
title: ipv6服务器踩坑总结
date: 2026-06-25 00:32:59
categories: [杂谈]
tags: [ipv6]
cover: https://qncdn.178981.xyz/test/20260625005954169.png?imageslim
---


## ipv6服务器踩坑总结

ipv6服务器是便宜，但是玩起来还是很蛋疼，很多坑，记录一下

### 1.docker容器开启ipv6

docker容器默认是关闭ipv6的，需要手动开启

#### 宝塔设置docker的ipv6
在宝塔的docker设置中，找到ipv6设置，开启ipv6
然后配置ipv6地址，本机ipv6后面添加/64，比如本机ipv6是`2409:8a00:3:2::1`，那么容器ipv6地址就是`2409:8a00:3:2::1/64`
然后再在docker网络里添加ipv6网络
添加的方法和1panel一样

#### 1panel设置docker的ipv6

用1panel面板，对docker的支持比较友好，设置ipv6相对简单，在ipv6设置里设置ipv6地址+/64，勾选ip6tables和experimental
然后在网络中创建一个ipv6的docker网络，因为我是纯IPV6服务器，就不设置ipv4地址了，只需要填写ipv6的子网就行了，子网是ip地址少最后一段
比如

```
xxxx:xxxx:8x:xxxx::22d #假如这是我的ipv6地址（随便编的）

#可以使用不同的 /64 子网（隔离）
xxxx:xxxx:8x:xxx2::/64
xxxx:xxxx:8x:xxx3::/64
...
2404:8c81:84:8910::/64
#随意使用任意子网段
```

### docker使用host模式

在宝塔和1panel中，docker的host模式使用有时候无效，建议在代码里设置host模式
在docker-compose.yml中设置
```
network_mode: host
```
这样更好，因为docker的host模式，容器和宿主机共享网络，这样设置，容器和宿主机网络是互通的，不需要设置端口映射，也不需要设置ipv6网络

### node打包成docker镜像

在node项目中，RUN npm ci --omit=dev这个设置，可能会因为网络原因，导致无法下载依赖，ai给的建议是直接把node_modules打包到镜像里，这样就不会因为网络问题导致打包失败

```
COPY node_modules ./node_modules
COPY . .
```

### 数据库链接

这也是个坑，有的服务器不识别ipv4地址导致数据库链接失败，需要使用ipv6的地址，比如
数据库链接，如果使用ipv6，需要使用`[::1]`，而不是`127.0.0.1`，否则会链接失败

```
::1
```

### 面板使用终端

在宝塔面板或者1panel面板中，使用终端，有概率会无法使用
建议还是用ssh工具更稳定

### 总结
ipv6服务器，确实比ipv4服务器要麻烦一些，但是只要配置好，使用起来还是很方便的，希望我的总结能帮到大家，后续有什么坑，我会继续更新