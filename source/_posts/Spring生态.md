---
title: Spring生态
date: 2025-02-17 14:09:41
tags: [Java, Spring, Spring Boot, Mybatis, Spring Cloud, Spring Security]
categories: Java
---


## 一、Spring Boot

### 1. 核心注解

- @Component: 标记一个类为 Spring 容器管理的组件，通用的注解。
- @Service: 标记一个类为服务层组件，通常用于业务逻辑层。
- @Repository: 标记一个类为数据访问层组件，通常用于 DAO 层。
- @Controller: 标记一个类为控制器组件，通常用于 MVC 模式中的控制器。
- @RestController: 结合了 @Controller 和 @ResponseBody，用于 RESTful Web 服务。

### 2. 依赖注入

- @Autowired: 自动注入依赖，可以用于字段、构造器或方法。
- @Qualifier: 与 @Autowired 一起使用，指定要注入的 bean 的名称。
- @Resource: 类似于 @Autowired，但按名称注入。
- @Value: 注入属性值，通常用于从配置文件中读取值。

### 3. 配置

- @Configuration: 标记一个类为配置类，通常与 @Bean 一起使用。
- @Bean: 标记一个方法返回的对象为 Spring 容器管理的 bean。
- @ComponentScan: 指定 Spring 扫描组件的包路径。
- @PropertySource: 指定属性文件的位置，用于加载配置。
  
### 4. AOP（面向切面编程）

- @Aspect: 标记一个类为切面类。
- @Before: 在目标方法执行前执行。
- @After: 在目标方法执行后执行，无论是否抛出异常。
- @AfterReturning: 在目标方法成功返回后执行。
- @AfterThrowing: 在目标方法抛出异常后执行。
- @Around: 环绕通知，可以在目标方法执行前后执行自定义逻辑。
  
### 5. 事务管理

- @Transactional: 标记一个方法或类为事务性的，用于声明式事务管理。
  
## 二、Mybatis

### 1. 什么是MyBatis？

MyBatis是一个基于Java的持久层框架，它封装了JDBC操作，简化了数据库访问。MyBatis通过XML或注解将Java对象与SQL语句进行映射，开发者可以直接编写SQL语句，灵活地控制查询逻辑。

### 2. MyBatis的核心组件有哪些？

- SqlSessionFactory：用于创建SqlSession的工厂类，是MyBatis的核心对象。
- SqlSession：表示一次数据库会话，用于执行SQL语句、获取Mapper接口。
- Mapper接口：定义了数据库操作的方法，MyBatis通过动态代理实现接口。
- Mapper XML文件：定义了SQL语句和结果映射规则。
- Configuration：包含了MyBatis的全局配置信息。
  
### 3. MyBatis中#{}和${}的区别是什么？

- #{}：
	- 是预编译占位符，MyBatis会将#{}替换为?，并通过PreparedStatement设置参数，防止SQL注入。
	- 适用于传递参数值。
- ${}：
	- 是字符串替换，MyBatis会直接将${}替换为变量的值，存在SQL注入风险。
	- 适用于动态表名、列名等场景。
	  
### 4. MyBatis如何实现分页？

- 逻辑分页：使用MyBatis的RowBounds对象，在内存中进行分页，适合数据量小的场景。
- 物理分页：在SQL语句中使用LIMIT和OFFSET（MySQL）或ROWNUM（Oracle）实现分页。
- 分页插件：使用PageHelper等分页插件，简化分页操作。

### 5. MyBatis的一级缓存和二级缓存有什么区别？

- 一级缓存：
	- 默认开启，作用域为SqlSession级别。
	- 在同一个SqlSession中，相同的查询会直接从缓存中获取结果。
	- 当SqlSession关闭或执行增删改操作时，缓存会被清空。
- 二级缓存：
	- 需要手动开启，作用域为Mapper级别（跨SqlSession）。
	- 多个SqlSession可以共享二级缓存。
	- 适合读取频繁、更新较少的场景。

### 6. MyBatis如何实现多表关联查询？

- 嵌套查询：在主查询中通过< association >或< collection >标签嵌套子查询。
- 嵌套结果：通过JOIN查询一次性获取所有数据，然后在结果映射中使用< association >或< collection >标签映射关联对象。

### 7. MyBatis的动态SQL有哪些标签？

- < if >： 条件判断。
- < choose >、< when >、< otherwise >： 多条件选择。
- < where >： 动态生成WHERE子句。
- < set >： 动态生成SET子句。
- < foreach >： 遍历集合，生成IN语句或批量操作。
- < trim >： 去除多余的字符（如AND、OR）。

### 8. MyBatis如何实现批量插入？

- 使用< foreach >标签遍历集合，生成多条INSERT语句。
- 使用SqlSession的batch方法实现批量操作。

### 9. MyBatis如何解决SQL注入问题？

- 使用#{}预编译占位符，避免直接拼接SQL。
- 避免使用${}进行字符串替换，除非是动态表名、列名等场景。
- 对用户输入进行严格的校验和过滤。

## 三、Spring Cloud

### 1. 服务注册与发现

- @EnableEurekaClient: 启用 Eureka 客户端，将服务注册到 Eureka 服务器。
- @EnableDiscoveryClient: 启用服务发现客户端，适用于多种服务发现工具（如 Eureka、Consul、Zookeeper）。
- @LoadBalanced: 标记 RestTemplate 或 WebClient，使其具备负载均衡能力。

### 2. 配置管理

- @EnableConfigServer: 启用配置服务器，集中管理微服务的配置。
- @RefreshScope: 标记 Bean，使其在配置更改时能够动态刷新。

### 3. API 网关

- @EnableZuulProxy: 启用 Zuul 代理，用于 API 网关路由和过滤。
- @EnableZuulServer: 启用 Zuul 服务器，提供基本的网关功能。
- @EnableGateway: 启用 Spring Cloud Gateway，提供更强大的 API 网关功能。

### 4. 熔断器与限流

- @EnableHystrix: 启用 Hystrix 熔断器，提供容错和延迟容忍能力。
- @HystrixCommand: 标记方法，使其具备熔断器功能。
- @EnableCircuitBreaker: 启用熔断器功能，适用于多种熔断器实现（如 Hystrix、Resilience4j）。
- @SentinelResource: 标记方法，使其具备 Sentinel 的限流和熔断功能。

### 5. 分布式追踪

- @EnableSleuth: 启用 Sleuth，提供分布式追踪功能。
- @NewSpan: 标记方法，使其在追踪中创建一个新的 Span。

### 6. 消息驱动

- @EnableBinding: 启用消息绑定，将应用程序与消息中间件（如 Kafka、RabbitMQ）连接。
- @StreamListener: 标记方法，使其监听消息通道中的消息。

## 四、Spring Security

### 1.@EnableWebSecurity

- 启用 Spring Security 的 Web 安全支持。
- 通常用于配置类上，表示该类是 Spring Security 的配置类。

### 2.@PreAuthorize

- 在方法执行前进行权限检查。
- 支持 SpEL（Spring Expression Language）表达式。

### 3.@Secured

- 用于指定方法所需的角色。
- 不支持 SpEL，仅支持简单的角色名称。

### 4.@WithAnonymousUser

- 用于测试，模拟匿名用户。