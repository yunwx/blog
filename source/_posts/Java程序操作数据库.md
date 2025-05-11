---
title: Java程序操作数据库
date: 2025-02-13 22:04:05
tags: [JDBC, Mybatis, Java]
categories: Java
---

## 一、JDBC

#### 1.介绍：

- 使用Java语言操作关系型数据库的一套API

#### 2.本质：

- sun公司官方定义的一套操作所有关系型数据库的规范，即接口
- 各个数据库厂商去实现这套接口，提供数据库驱动jar包
- 我们可以使用这套接口（JDBC）编程，真正执行的代码是驱动jar包中的实现类

#### 3.步骤：

（1）注册驱动

``` java
Class.forName("com.mysql.cj.jdbc.Driver");
```

（2）获取连接

``` java
String url = "jdbc:mysql://IP地址:端口号/数据库名";
String username = "用户名";
String password = "密码";
Connection connection = DriverManager.getConnection(url,username,password);
```

（3）获取SQL语句执行对象

``` java
Statement statement = connection.createStatement();
```

（4）执行SQL语句

- DML语句：

	``` java
	int i = statement.executeUpdate("DML语句");
	System.out.println("影响的记录数为：" + i);
	```

- DQL语句：

	``` java
	ResultSet rs = statement.executeQuery("DQL语句");
	```

（5）释放资源

``` java
statement.close();
connection.close();
```

#### 4.查询数据：

- ResultSet（结果集对象）：ResultSet rs = statement.executeQuery()
	- next()：将光标从当前位置向前移动一行，并判断当前行是否为有效行，返回值为boolean
		- true：有效行，当前行有数据
		- false：无效行，当前行没有数据
	- getXxx(...)：获取数据，可以根据列的编号获取，也可以根据列名获取（推荐）
	``` java
	rs.getInt("ID");
	rs.getString("USERNAME");
	```

#### 5.预编译SQL（参数动态传递）：

- 优势：
	- 可以防止SQL注入，更安全
		- SQL注入：通过控制输入来修改事先定义好的SQL语句，以达到执行代码对服务器进行攻击的方式
	- 性能更高
		- SQL语句先编译后执行，性能更高
	- 类型安全
		- 强类型的setXxx(...)方法
- 执行：
	- PreparedStatement是Statement子接口（专门用于预编译SQL）
	- ?：预编译占位符：有几个就需要调用set方法赋值几个
	- setXxx(...) ：
		- 给预编译占位符赋值
		- <span style="color:#FF6B6B">索引从 1 开始（不是 0！）</span>
``` java
PreparedStatement ps = connection.prepareStatement("SELECT * FROM user WHERE username = ? AND password = ?");
ps.setString(1,"LiHua");
ps.setString(2,"123456");
ResultSet rs = ps.executeQuery();
```

## 二、MyBatis

#### 1.介绍：

- 一款优秀的持久层框架，用于简化JDBC的开发

#### 2.配置：

- 在 resources 的 application.properties 中配置数据库连接信息

``` java
spring.datasource.url=jdbc:mysql://IP地址:端口号/数据库名
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.username=用户名
spring.datasource.password=密码
```

#### 3.编写Mybatis程序：

- 编写Mybatis的持久层接口，定义SQL（注解/XML）
	- **@Mapper**：应用程序在运行时，会自动的为该接口创建一个实现类对象（代理对象），并且会自动将该实现类对象存入IOC容器（bean）
	- **@Select**：查询
		- 例：
	``` java
	@Mapper
	public interface UserMapper {
		@Select("select * from user")
		public List<User> findAll(); //User为已定义的实体类，查询到的结果会自动封装到List<User>中
    
		@Select("select * from user where username=#{username} and password=#{password}")
		public User findByUsernameAndPassword(@Param("username") String username,@Param("password") String password);
	}
	```

	- <span style="color:#FF6B6B">注：</span>
		- <span style="color:#FF6B6B">@Param注解的作用是为接口方法的形参起名字（接口编译时不会保留形参名，多个参数情况下需要使用@Param）</span>
		- <span style="color:#FF6B6B">基于官方骨架创建的springboot项目中，接口编译时会保留方法形参名，@Param注解可以省略</span>
	- **@Insert**：插入
		- 例：
	``` java
	@Insert("insert into user(username,password,name,age) values(#{username},#{password},#{name},#{age})")
	public void insert(User user); //#{username},#{password},#{name},#{age}是user对象的属性名
	```

	- <span style="color:#FF6B6B">注：如果需要传递的参数过多，可以将其封装到对象中</span>
	- **@Update**：修改
		- 例：
	``` java
	@Update("update user set username=#{username},password=#{password},name=#{name},age=#{age} where id=#{id}")
	public void update(User user);
	```

	- **@Delete**：删除
		- 例：
	``` java
	@Delete("delete from user where id = #{id}")
	public Integer deleteById(Integer id);
	```

	- <span style="color:#FF6B6B">注：执行DML（数据操作）语句时返回值为 int 类型，表示DML语句执行影响的记录数</span>


- 调用
	- 例：
``` java
@Autowired
private UserMapper userMapper;

List<User> userList = userMapper.findAll();
```

#### 4.辅助配置：

- 在Mybatis中查看SQL语句的执行日志（控制台输出）
	- 在 resources 的 application.properties 中配置
``` java
mybatis.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
```

#### 5.数据库连接池：

- 介绍：
	- 数据库连接池是个容器，负责分配、管理数据库连接（Connection）
	- 它允许应用程序重复使用一个现有的数据库连接，而不是再重新建立一个
	- 释放空闲时间超过最大空闲时间的连接，来避免因为没有释放连接而引起的数据库连接遗漏
- 优势：
	- 资源重用
	- 提升系统响应速度
	- 避免数据库连接遗漏
- 标准接口：DataSource
	- 官方（sun）提供的数据库连接池接口，由第三方组织实现此接口
	- 功能：
		- 获取连接：
	``` java
	Connection getConnection() throws SQLException;
	```

- 切换数据库连接池：
	- SpringBoot默认数据库连接池为Hikari
	- 在 resources 的 application.properties 中配置
	``` java
	spring.datasource.type=com.alibaba.druid.pool.DruidDataSource //切换为德鲁伊连接池，是阿里巴巴开源的数据库连接池项目（需要引入依赖）
	```

#### 6. # 号与 $ 号：

- #{...}：
	- 说明：占位符。执行时会将#{...}替换为？，生成预编译SQL
	- 场景：参数值传递
	- 特点：安全、性能高
- ${...}：
	- 说明：拼接符。直接将参数拼接在SQL语句中，存在SQL注入问题
	- 场景：表名、字段名动态设置时使用
	- 特点：不安全、性能低

#### 7.XML映射配置：

- 在Mybatis中，既可以通过注解配置SQL语句，也可以通过XML配置文件配置SQL语句
- 默认规则：
	- XML映射文件的名称必须与Mapper接口名称一致，并且将XML映射文件和Mapper接口放置在相同包下（同包同名）
	- XML映射文件的namespace属性为Mapper接口全限定名
	- XML映射文件中SQL语句的id与Mapper接口中的方法名一致，并保持返回类型一致
	- 例：
		- 项目结构：
		{% asset_img 1.jpg %}
		- 配置：
	``` xml
	<mapper namespace="com.itheima.mapper.UserMapper">
		<select id="findAll" resultType="com.itheima.pojo.User">
			select id,username,password,name,age from user
		</select>
	</mapper>
	```

<span style="color:#FF6B6B">注：使用Mybatis的注解主要是来完成一些简单的增删改查功能。如果需要实现复杂的SQL功能，建议使用XML来配置映射语句</span>
- 辅助配置：
	- 配置XML映射文件的位置：
		- 在 resources 的 application.properties 中配置
	``` java
	//classpath表示类路径（是java和resources的集合），mapper为目录名，*.xml是在其目录（mapper）下查询所有XML文件
	mybatis.mapper-locations=classpath:mapper/*.xml
	```

	- MybatisX：
		- 是一款基于IDEA的快速开发Mybatis的插件，为效率而生