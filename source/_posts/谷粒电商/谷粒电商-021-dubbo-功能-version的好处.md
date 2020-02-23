---
title: 谷粒电商-021-dubbo-功能-version的好处
date: 2020-02-18 11:09:18
tags:
    - 谷粒电商
    - Dubbo
---

# dubbo:reference 配置中
关于version的妙用

provider
```
@Component // 本工程内Spring组件可以Autowired
@Service(version = "1.0") // 使用dubbo的，将此服务暴露，为了别的工程能远程调
public class MovieServiceImpl implements MovieService {

    @Override
    public Movie getNewMovie() {

        Movie movie = new Movie();
        movie.setId(1);
        movie.setMovieName("复联4");
        movie.setPrice(96.99);
        System.out.println("20880--v1.0, 电影服务查询到最新的电影: "+movie.getMovieName());

        try {
            Thread.sleep(4000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        return movie;
    }
}
```

consumer
```
@Service  // spring
public class UserServiceImpl implements UserService {

    // 只需声明
    @Reference(version = "1.0") // dubbo
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

可以让大部分consumer使用verion 1.0的集群，少部分consumer使用version 2.0的集群，
这样，当versioin 2.0出现问题，排除故障后，保证v2.0稳定了，再系统全面升级为v2.0。

注：v1.0和v2.0对于consumer不能互通。

# 参考
http://dubbo.apache.org/zh-cn/docs/user/references/xml/dubbo-reference.html
