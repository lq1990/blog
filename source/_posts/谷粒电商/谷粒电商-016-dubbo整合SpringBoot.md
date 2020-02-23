---
title: 谷粒电商-016-dubbo整合SpringBoot
date: 2020-02-17 14:07:37
tags: 
    - 谷粒电商
    - Dubbo
    - Spring Boot
---

# dubbo整合SpringBoot
在之前的项目中，Spring整合provider和consumer做配置很麻烦。

改进：
使用SpringBoot

## 步骤
1. 新建模块 dubbo-movie-provider-boot
2. 新建模块 dubbo-user-consumer-boot
   
3. 在父项目dubbo-api的pom.xml中，导入
```
<dependencies>
    <!--<dependency>-->
        <!--<groupId>com.alibaba</groupId>-->
        <!--<artifactId>dubbo</artifactId>-->
        <!--<version>2.6.2</version>-->
    <!--</dependency>-->

    <!--&lt;!&ndash;dubbo2.6+ 使用curator操作zk&ndash;&gt;-->
    <!--<dependency>-->
        <!--<groupId>org.apache.curator</groupId>-->
        <!--<artifactId>curator-recipes</artifactId>-->
        <!--<version>2.12.0</version>-->
    <!--</dependency>-->

    <dependency>
        <groupId>com.alibaba.boot</groupId>
        <artifactId>dubbo-spring-boot-starter</artifactId>
        <version>0.2.0</version>
    </dependency>

</dependencies>
```

4. 在provider-boot和consumer-boot两个项目中的pom.xml 导入父项目dubbo-api依赖。

5. provider-boot项目的application.properties中配置
```
dubbo.application.name=dubbo-movie-provider-boot
dubbo.registry.address=zookeeper://39.105.96.93:2181
dubbo.protocol.port=20880

# 有很多种协议，dubbo默认使用dubbo协议。cloud使用的是http协议。
# http://dubbo.apache.org/zh-cn/docs/user/references/protocol/dubbo.html
dubbo.protocol.name=dubbo
```

6. 当provider需要暴露的服务很多时，为了方便，使用注解
```
import com.alibaba.dubbo.config.annotation.Service;

/**
 *
 * @author lq
 * create 2020-02-16 12:34
 */
@Service // 使用dubbo的，将此服务暴露
public class MovieServiceImpl implements MovieService {

    @Override
    public Movie getNewMovie() {

        Movie movie = new Movie();
        movie.setId(1);
        movie.setMovieName("复联4");
        movie.setPrice(96.99);
        System.out.println("电影服务查询到最新的电影: "+movie.getMovieName());

        return movie;
    }
}
```

7. 开启dubbo基于注解的功能 @EnableDubbo
```
@EnableDubbo
@SpringBootApplication
public class DubboMovieProviderBootApplication {

    public static void main(String[] args) {
        SpringApplication.run(DubboMovieProviderBootApplication.class, args);
    }

}
```
8. 启动provider App

9. consumer的配置
application.properties
```
dubbo.application.name=dubbo-user-consumer-boot
dubbo.registry.address=zookeeper://39.105.96.93:2181
server.port=8081
```

```
import com.alibaba.dubbo.config.annotation.Reference;
import org.springframework.stereotype.Service;

/**
 * @author lq
 * create 2020-02-16 12:07
 */
@Service  // spring
public class UserServiceImpl implements UserService {

    // 只需声明
    @Reference // dubbo
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

10.  测试，创建controller
```
@Controller
public class HelloController {

    @Autowired
    UserService userService;

    @ResponseBody
    @RequestMapping("/hello")
    public Object hello() {
        Order order = userService.buyNewMovie(new User(1, "anna"));

        return order;
    }
}
```
在consumer的App上添加注解 @EnableDubbo
运行provider, consumer主类
访问 localhost:8081/hello

## 总结
provider的dubbo的 @Service注解，暴露provider某些服务
consumer的 @Reference注解，对服务进行引用