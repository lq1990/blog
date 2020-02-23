---
title: 谷粒电商-011-Dubbo-提供者-消费者-分包
date: 2020-02-16 10:53:42
tags: 
    - 谷粒电商
    - Dubbo
---

# 快速启动
Dubbo采用Spring配置方式，透明化接入应用，对应用没有任何API侵入，只需要Spring加载Dubbo的配置即可，Dubbo基于Spring的Schema扩展进行加载。

如果不想使用Spring配置，可以通过API的方式进行调用。

## 服务提供者 Provider
IDEA - 创建空工程 - 新建多个modules

- dubbo-movie-provider
- dubbo-user-consumer
  
### 问题：
两个项目之间相互调用的问题
### 解决：
另外新建一个工程
- dubbo-api

把**javabean，接口，服务模型，服务异常**等公共部分都放到此工程中。
这样，其它工程导入此工程依赖即可。

### 问题：
在consumer工程内使用某个service接口，调用接口中方法，在运行项目时会空指针异常，因为调用的是接口而非实现类。

```
public class UserServiceImpl implements UserService {

    // 只需声明
    MovieService movieService;

    @Override
    public Order buyNewMovie(User user) {
        // 1. 远程查询最新电影并返回
        Movie newMovie = movieService.getNewMovie();
        System.out.println("远程调用movie服务获取到结果: "+newMovie);

        // 2. 封装order对象返回
        Order order = new Order();
        order.setUserName(user.getUserName());
        order.setMovieId(newMovie.getId());
        order.setMovieName(newMovie.getMovieName());
        return order;
    }
}
```

### 解决：
使用dubbo。
    


