# SpringBoot

**SpringBoot配置文件**
* application.properties
```java
//变量引用
app.name=MyApp
app.description=${app.name} is a Spring Boot application
```
* 加载 YAML

Spring Framework 提供了两个便捷类，可用于加载 YAML 文档。YamlPropertiesFactoryBean 将 YAML 加载为 Properties，YamlMapFactoryBean 将 YAML 加载为 Map
```java
environments:
  dev:
    url: http://dev.example.com
    name: Developer Setup
  prod:
    url: http://another.example.com
    name: My Cool App
```
* YAML 列表表示带有 `[index]` 下标引用的属性键。例如以下 YAML：
```java
my:
servers:
  - dev.example.com
  - another.example.com

//通过注解获取YMl值
@Value("${my.servers[0]}")
private String ymlValue;
```
以上示例将转成以下属性：
```java
my.servers[0]=dev.example.com
my.servers[1]=another.example.com

//通过注解
@Value("${my.servers[0]}")
private String propertiesValue;
//application.properties 值获取
@ConfigurationProperties(prefix="my")
public class Config {
	
	private List<String> servers = new ArrayList<String>();

    public List<String> getServers() {
        return this.servers;
    }

}

```
* 多 profile YAML 文档


```java
server:
  address: 192.168.1.100
---
spring:
  profiles: development
server:
  address: 127.0.0.1
---
spring:
  profiles: production & eu-central
server:
  address: 192.168.1.120
```
在前面示例中，如果 `development profile` 处于激活状态，则 `server.address` 属性得值为 `127.0.0.1`。 同样，如果 `production` 和 `eu-central` `profile` 处于激活状态，则 `server.address` 属性的值为 `192.168.1.120`。如果未激活 `development`、`production` 或 `eu-central` `profile`，则该属性的值为 `192.168.1.100`。


https://docshome.gitbooks.io/springboot/
