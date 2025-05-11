---
title: Lombok库
date: 2025-01-28 20:56:18
tags: [Java, Lombok]
categories: Java
---

## 一、简介

- Lombok是一个Java库，通过注解的方式简化Java代码的编写，减少样板代码，提高开发效率。它通过注解处理器在编译时自动生成代码，如getter/setter、构造函数、equals/hashCode等方法。
- 优点：
	- 减少样板代码，使类更简洁
	- 提高开发效率
	- 自动生成的代码更规范
	- 减少人为错误

## 二、常用注解

#### 1.简化POJO类

(1) **@Data**：
- 是一个复合注解，包含以下功能：
- 自动生成所有字段的 getter 方法
- 生成非 final 字段的 setter 方法
- 生成 equals() 和 hashCode() 方法
- 生成 toString() 方法
- 相当于 @Getter @Setter @ToString @EqualsAndHashCode 的组合
- 例：

``` java
@Data
public class User {
    private Long id;
    private String name;
}
```

(2) **@NoArgsConstructor**：
- 自动生成无参构造函数
- 如果类中有 final 字段且未初始化，需要配合 @NoArgsConstructor(force = true) 使用
- force=true时会给final字段赋默认值(null/0/false)
- 例：

``` java
@NoArgsConstructor
public class Example {
    private final String name; // 编译错误，需要@NoArgsConstructor(force = true)
}
```

(3) **@AllArgsConstructor**：
- 自动生成包含所有字段的构造函数
- 字段顺序与类中声明的顺序一致
- 不包含static和final未初始化的字段
- 例：

``` java
@AllArgsConstructor
public class Product {
    private Long id;
    private String name;
    private double price;
}
// 等效于：
public Product(Long id, String name, double price) {
    this.id = id;
    this.name = name;
    this.price = price;
}
```

(4) **@RequiredArgsConstructor**：
- 生成包含特定字段的构造函数
- 包含final字段和带有@NonNull注解的未初始化字段
- 例：

``` java
@RequiredArgsConstructor
public class Order {
    private final Long orderId;
    @NonNull
    private String customerName;
    private double amount;
}
// 生成的构造函数：
public Order(Long orderId, String customerName) {
    this.orderId = orderId;
    this.customerName = customerName;
}
```

#### 2.简化资源管理

(1) **@Cleanup**：
- 自动管理资源关闭
- 用于需要关闭的资源对象（如IO流、数据库连接等）
- 在变量作用域结束时自动调用close()方法
- 例：

``` java
public void copyFile(String src, String dest) throws IOException {
    @Cleanup InputStream in = new FileInputStream(src);
    @Cleanup OutputStream out = new FileOutputStream(dest);
    byte[] buffer = new byte[1024];
    int length;
    while ((length = in.read(buffer)) != -1) {
        out.write(buffer, 0, length);
    }
}
// 无需手动调用 in.close() 和 out.close()
```

#### 3. 简化日志配置

(1) **@Slf4j**：
- 自动生成SLF4J日志对象
- 变量名为log
- 例：

``` java
@Slf4j
public class OrderService {
    public void processOrder() {
        log.info("Processing order..."); // 直接使用log对象
    }
}
```

(2) 其他日志注解：
{% asset_img 1.jpg %}  

#### 4. 其他实用功能

(1) **@Builder**：
- 实现建造者模式
- 生成builder()静态方法
- 支持设置默认值（@Builder.Default）
- 例：

``` java
@Builder
public class Product {
    private Long id;
    @Builder.Default
    private String name = "default";
}
// 使用方式：
Product product = Product.builder()
    .id(1L)
    .build(); // name默认为"default"
```

(2) **@SneakyThrows**：
- 偷偷抛出受检异常（无需声明throws）
- 常用于不想处理受检异常的场景
- 例：

``` java
@SneakyThrows(IOException.class)
public String readFile(String path) {
    return Files.readString(Paths.get(path));
}
// 调用处不需要处理IOException
```

(3) **@Synchronized**：
- 方法级同步的更安全实现
- 比synchronized关键字更细粒度
- 生成私有锁对象避免死锁
- 例：

``` java
public class Counter {
    @Synchronized
    public void increment() {
        // 线程安全操作
    }
}
```

(4) **@Value**：
- 创建不可变类（所有字段默认final）
- 包含@Getter、@ToString、@EqualsAndHashCode等
- 不生成setter
- 例：

``` java
@Value
public class ImmutablePoint {
    int x;
    int y;
}
// 生成的类相当于：
public final class ImmutablePoint {
    private final int x;
    private final int y;
    // 只有getter，没有setter
}
```

(5) **@With**：
- 创建对象的不可变副本并修改指定字段
- 适用于不可变对象的"修改"操作
- 例：

``` java
@With
@AllArgsConstructor
public class Person {
    private String name;
    private int age;
}
// 使用方式：
Person p1 = new Person("Alice", 30);
Person p2 = p1.withAge(31); // 创建新对象，只修改age
```

