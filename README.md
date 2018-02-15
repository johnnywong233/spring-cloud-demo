## 项目地址汇总
### Spring Cloud Demo：[https://github.com/roncoo/spring-cloud-demo](https://github.com/roncoo/spring-cloud-demo)
### Spring Boot Demo：[https://github.com/roncoo/spring-boot-demo](https://github.com/roncoo/spring-boot-demo)

### 本项目为Spring cloud的基础教程

- 教程视频：[免费篇](http://www.roncoo.com/course/view/e4189c9db6474745b5e578983cddd112)
- 教程视频：[Spring boot全集](http://www.roncoo.com/course/view/c99516ea604d4053908c1768d6deee3d#boxTwo)
- 教程视频：[Srping cloud第一季](http://www.roncoo.com/course/view/cc8fbd6749f94f2fa015641ef96b9460#boxTwo)

### 项目文档
- spring-boot-demo [教程文档](http://www.roncoo.com/article/detail/124661)

### 项目说明
- 源码只供学习使用，更多请看视频

### 技术交流
* QQ2群: 601146630
* QQ1群: 213097382 (满)

## 开源项目 
### roncoo-jui-springboot：[https://github.com/roncoo/roncoo-jui-springboot](https://github.com/roncoo/roncoo-jui-springboot)
- 该项目是为了大家更好地运用Spring Boot的功能，进行实战。
- 如果没有使用过Spring Boot，也是一个学习的好项目。

## 项目结构
- consumer：服务消费者，注解 @EnableEurekaClient
- service：服务注册中心，注解 @EnableEurekaServer
- provider：服务提供者，注解 @EnableEurekaClient

## 配置文件
```
# eureka
# 是否注册到eureka
eureka.client.register-with-eureka=false
# 是否从eureka获取注册信息
eureka.client.fetch-registry=false
# eureka服务器的地址（注意：地址最后面的 /eureka/ 这个是固定值）
eureka.client.serviceUrl.defaultZone=http://localhost:${server.port}/eureka/


# info自定义
info.build.name=@project.name@
info.build.description=@project.description@
info.build.groupId=@project.groupId@
info.build.artifact=@project.artifactId@
info.build.version=@project.version@

eureka.instance.status-page-url-path=/info
eureka.instance.instanceId=${spring.application.name}:${random.value}
eureka.instance.prefer-ip-address=true

#设置拉取服务注册信息时间，默认60s
eureka.client.registry-fetch-interval-seconds=30

#指定续约更新频率，默认是30s
eureka.instance.lease-renewal-interval-in-seconds=15

#设置过期剔除时间，默认90s
eureka.instance.lease-expiration-duration-in-seconds=45


# 指定环境
eureka.environment=dev

#指定数据中心
eureka.datacenter=roncoo

# 关闭自我保护模式
eureka.server.enable-self-preservation=false

#设置清理无效节点的时间间隔，默认60000，即是60s
eureka.server.eviction-interval-timer-in-ms=5000



```

## tip
- 使用 spring-boot-security-starter，配置文件加
```
# 服务认证
security.basic.enabled=true
security.user.name=roncoo
security.user.password=123456
```
- 

## spring-cloud-05
演示注册中心的高可用，copy 三个 eureka-server，配置文件指向另外两个 server 的地址，示例：
```
eureka.instance.hostname=roncoo3
eureka.client.serviceUrl.defaultZone=http://roncoo:123456@roncoo1:8761/eureka/,http://roncoo:123456@roncoo2:8762/eureka/
```

## spring-cloud-06
演示服务提供者的高可用，copy 三个 @EnableEurekaClient 注解启动类，保持提供的 controller 接口的的 request url 一样，
然后在 consumer 工程里，加上配置 bean：
```
@Bean
@LoadBalanced
protected RestTemplate restTemplate() {
    return new RestTemplate();
}
```