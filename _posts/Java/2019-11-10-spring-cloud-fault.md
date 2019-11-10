---
title: "Spring Cloud 之殇"
subtitle: "盲目崇拜即是无知"
layout: post
author: "tokenian"
# header-style: text
header-img: "img/home-bg-f.jpg"
tags:
  - java
  - spring cloud
  - source code
---

如果你打开一些互联网招聘网站，搜索Java的职位，对于spring 系列的技能成了必备。spring/spring boot/spring cloud,三驾马车，缺了一样都不行。是的，微服务开发模式最近几年已经逐渐替代了以前的单体应用/SOA/WebService等。

我使用spring cloud 也有一段时间了，阅读过它的一部分源码。从使用体验上来讲，还是spring > spring boot > spring cloud。 spring cloud的代码里充斥着源文件里直接写单元测试，变量命名不规范，注释不加等诸多问题。我并不是想批判它，它作为星星之火，点燃了微服务开发的荒原。我只想说，开源的世界很美好，没必要盲目崇拜。以下，我会列举一些工作中遇到的使用问题。

首先声明使用的版本， spring boot@2.1.5.RELEASE, spring cloud@Greenwich.SR1

### 服务注册发现(Service Discovery)
在eureka的`DiscoveryClient`中,有一个方法`getAndStoreFullRegistry`,它会从注册服务器拿到所有的注册实例的信息,保存到本地。
```
   /**
     * Gets the full registry information from the eureka server and stores it locally.
     * When applying the full registry, the following flow is observed:
     *
     * if (update generation have not advanced (due to another thread))
     *   atomically set the registry to the new registry
     * fi
     *
     * @return the full registry information.
     * @throws Throwable
     *             on error.
     */
    private void getAndStoreFullRegistry() throws Throwable {
        long currentUpdateGeneration = fetchRegistryGeneration.get();

        logger.info("Getting all instance registry info from the eureka server");

        Applications apps = null;
        EurekaHttpResponse<Applications> httpResponse = clientConfig.getRegistryRefreshSingleVipAddress() == null
                ? eurekaTransport.queryClient.getApplications(remoteRegionsRef.get())
                : eurekaTransport.queryClient.getVip(clientConfig.getRegistryRefreshSingleVipAddress(), remoteRegionsRef.get());
        if (httpResponse.getStatusCode() == Status.OK.getStatusCode()) {
            apps = httpResponse.getEntity();
        }
        logger.info("The response status is {}", httpResponse.getStatusCode());

        if (apps == null) {
            logger.error("The application is null for some reason. Not storing this information");
        } else if (fetchRegistryGeneration.compareAndSet(currentUpdateGeneration, currentUpdateGeneration + 1)) {
            localRegionApps.set(this.filterAndShuffle(apps));
            logger.debug("Got full registry with apps hashcode {}", apps.getAppsHashCode());
        } else {
            logger.warn("Not updating applications as another thread is updating it already");
        }
    }
```
我们通常的服务调用使用的是Feign,Feign 底下是Ribbon，Ribbon IRule策略（负载均衡）从本地拿到的服务实例信息进行RestTemplate调用。那么问题来了

我们的注册中心注册了成百上千的服务，每个服务再有几个实例做高可用配置。如果当前的服务仅仅需要关注其中一个服务，那么其他的服务对它而言毫无价值，保存其他的服务到本地意义何在？

客户端默认每隔30s做一次心跳，上报健康状态。注册中心接受成百上千的客户端信息，是否有压力，这个策略是否太笨拙。注册中心以内存的方式保存注册信息是否撑得住？

### 熔断限流

spring cloud 对于熔断和限流提供的是Hystrix, 可以使用线程池和信号量。基于配置文件的配置相当复杂，有Feign的配置，Hystrix的配置，Ribbon的配置，对于某个参数的设置,可能在其中之一，也可能所有都可以配置。比如超时的设置，Hystrix里面有，Ribbon里也有。总之，使用起来煞费脑力。

Hystrix提供的熔断限流个人感觉不够灵活，感觉就像是你买了个毛坯房，得自己装修，你还的去学习下装修的知识。

既然spring推崇元编程的概念，通过annotation的方式。为什么不把限流/超时等的设置放到Feign的声明式标记呢。最后是针对具体的接口调用做声明

### Feign调用
Feign 对于同一个服务的不同实例，是不支持通过annotation的方式创建，必须手动创建注册到spring container.

feign对于fallback的实现缺乏灵活性，不满足于业务。

不同的服务，提供了不同的接口，有的可能安全性不敏感，常规的调用可以。有的数据安全敏感，需要鉴权。目前的Feign并不支持权限鉴权，应该通过annotation的方式，在调用的接口上声明。

### zuul网关
zuul的代码我大部分通读了，处理方式是自定义Servlet或者filter。定义了三个阶段， pre, route, post. 三个阶段对应于数据的请求前后和处理中，类似于spring mvc中的拦截器。

和Feign调用同样的问题，缺乏对路由的各个接口精细化控制，熔断限流能力太过笨拙。对不同的服务做网关，各个服务各个接口依然缺乏安全方面的控制。对于流量的复制功能（方便别人复现排查问题，重放）没有。

### 总结
以上是个人使用 的一些体会，欢迎留言讨论。毕竟工作更多关注于业务，可能了解的不全面。