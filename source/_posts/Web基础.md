---
title: Web基础
date: 2025-01-15 21:02:21
tags: [Java, HTTP]
categories: Java
---

## 一、基础概念

#### 1.静态资源：

- 服务器上存储的不会改变的数据，通常不会根据用户的请求而变化。比如：HTML、CSS、JS、图片、视频等（负责页面展示）

#### 2.动态资源：

- 服务器根据用户请求和其他数据动态生成的，内容可能会在每次请求时都发生变化。比如：Spring框架等（负责逻辑处理）

#### 3.B/S 架构：

- Browser/Server，浏览器/服务器架构模式。客户端只需浏览器，应用程序的逻辑和数据都存在服务器端（维护方便 体验一般）

#### 4.C/S 架构：

- Client/Server，客户端/服务器架构模式。需要单独开发维护客户端（体验不错 开发维护麻烦）

## 二、HTTP协议

#### 1.概念：

- 超文本传输协议，规定了浏览器和服务器之间数据传输的规则

#### 2.特点：

- 基于TCP协议：面向连接，安全
- 基于请求-响应模型：一次请求对应一次响应
- HTTP协议是无状态的协议：对于事务处理没有记忆能力。每次请求-响应都是独立的
	- 缺点：多次请求间不能共享数据
	- 优点：速度快

#### 3.请求协议：

- 请求数据格式：
	- 请求行：请求数据第一行，包含请求方式、资源路径、协议
	- 请求头：第二行开始，格式 key:value
		- 常见请求头
		{% asset_img 1.jpg %}
	- 请求体：POST请求存放请求参数
		- 请求方式-GET：请求参数在请求行中，没有请求体，GET请求大小在浏览器中是有限制的
		- 请求方式-POST：请求参数在请求体中，POST请求大小是没有限制的
<span style="color:#FF6B6B">注：请求头和请求体之间有一个空行</span>

- 请求数据获取：
	- Web服务器（Tomcat）对HTTP协议的请求数据进行解析，并进行了封装（HttpServletRequest），在调用Controller方法的时候传递给了该方法
	- 获取方法：
	{% asset_img 2.jpg %}
#### 4.响应协议：

- 响应数据格式：
	- 响应行：响应数据第一行，包含协议、状态码、描述
		- 状态码：
		{% asset_img 3.jpg %}
	- 响应头：第二行开始，格式 key:value
		- 常见响应头
		{% asset_img 4.jpg %}
	- 响应体：最后一部分，存放响应数据
<span style="color:#FF6B6B">注：响应头和响应体之间有一个空行</span>

- 响应数据设置：
	- Web服务器对HTTP协议的响应数据进行了封装（HttpServletResponse），并在调用Controller方法的时候传递给了该方法
	- 设置方法：
		- 方法一：
		{% asset_img 5.jpg %}
		- 方法二：
		{% asset_img 6.jpg %}
#### 5.其他：

- 静态资源文件存放位置（SpringBoot项目）：
	- resources/static
- @ResponseBody注解作用：
	- @RestController = @Controller + @ResponseBody
	- 将controller方法的返回值直接写入HTTP响应体
	- 如果是对象或集合，会先转为json，再响应