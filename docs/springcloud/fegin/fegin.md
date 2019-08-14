# 声明式服务调用（Fegin）

## 一. Feigin简介
      Feign是一个声明式的伪Http客户端，它使得写Http客户端变得更简单。使用Feign，只需要创建一个接口并用注解的方式来配置它，即可完成对服务提供方的接口绑定，简化了在使用Ribbon时自行封装服务调用客户端的开发量。

      Feign具有可插拔的注解特性，包括Feign 注解和JAX-RS注解，同时也扩展了对SpringMVC的注解支持。Feign支持可插拔的编码器和解码器，默认集成了Ribbon，并和Eureka结合，默认实现了负载均衡的效果。
