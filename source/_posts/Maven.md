---
title: Maven
date: 2025-01-25 22:59:17
tags: Maven
categories: Java
---

## 1.介绍：

- Maven是一款用于管理和构建Java项目的工具，是apache旗下的一个开源项目
- 官网：http://maven.apache.org/
{% asset_img 1.jpg %}
- 仓库：用于存储资源，管理各种jar包
	- 本地仓库：自己计算机上的一个目录
	- 中央仓库：由Maven团队维护的，全球唯一的。仓库地址：https://repo1.maven.org/maven2/
	- 远程仓库（私服）：一般由公司团队搭建的私有仓库
	<span style="color:#FF6B6B">注：查找依赖（jar）的顺序：本地仓库 -> 远程仓库 -> 中央仓库</span>

## 2.作用：

- 依赖管理：方便快捷的管理项目依赖的资源（jar包）
- 项目构建：标准化的跨平台（Linux、windows、MacOS）的自动化项目构建方式
- 统一项目结构：提供标准、统一的项目结构

## 3.安装：

（1）解压 apache-maven-3.9.4-bin.zip
（2）配置本地仓库：修改 conf/settings.xml 中的< localRepository >为一个指定目录

``` xml
<localRepository>目录路径</localRepository>
```

（3）配置阿里云私服：修改 conf/settings.xml 中的< mirrors >标签，为其添加如下子标签：

``` xml
<mirror>
    <id>alimaven</id>
    <name>aliyun maven</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
    <mirrorOf>central</mirrorOf>
</mirror>
```

（4）配置环境变量：MAVEN_HOME 为maven的解压目录，并将其bin目录加入PATH环境变量

## 4.坐标：

- 简介：
	- Maven 中的坐标是资源（jar）的唯一标识，通过该坐标可以唯一定位资源位置
	- 使用坐标来定义项目或引入项目中需要的依赖
- 主要组成：
	- groupId：定义当前Maven项目隶属组织名称（通常是域名反写，如com.baidu）
	- artifactId：定义当前Maven项目名称（通常是模块名称）
	- version：定义当前项目版本号
		- SNAPSHOT：功能不稳定、尚处于开发中的版本，即快照版本
		- RELEASE：功能趋于稳定、当前更新停止，可以用于发行的版本

## 5.导入Maven项目：

- 建议将要导入的maven项目复制到你的项目目录下
- 建议选择maven项目的pom.xml文件进行导入

## 6.依赖管理：

- 依赖配置：
	- 依赖：指当前项目运行所需要的jar包，一个项目中可以引入多个依赖
	- 配置：
		- 在 pom.xml 中编写< dependencies >标签
		- 在< dependencies >标签中使用< dependency >引入坐标
		- 定义坐标的 groupId, artifactId, version
		- 点击刷新按钮，引入最新加入的坐标
	<span style="color:#FF6B6B">注：如果不知道依赖的坐标信息，可以到 https://mvnrepository.com/ 中搜索</span>
	- 排除依赖：
		- 简介：指主动断开依赖的资源，被排除的资源无需指定版本
		- 配置：
			- < dependency >编写< exclusions >标签
			- 在< exclusions >标签中使用< exclusion >引入坐标
			- 定义坐标的 groupId, artifactId（无需指定版本）
	<span style="color:#FF6B6B">注：一旦依赖配置变更了，需要重新加载；引入的依赖如果本地仓库不存在，需要联网下载</span>
- 生命周期：
	- 作用：为了对所有的maven项目构建过程进行抽象和统一
	- 三套相互独立的生命周期：
		- clean：清理工作
		- default：核心工作：如：编译、测试、打包、安装、部署等
		- site：生成报告、发布站点等
	- 阶段
		- 每套生命周期包含一些阶段（phase），阶段是有顺序的，后面的阶段依赖于前面的阶段
		- 重要阶段：
			- clean：移除上一次构建生成的文件
			- compile：编译项目源代码
			- test：使用合适的单元测试框架运行测试（junit）
			- package：将编译后的文件打包，如：jar、war等
			- install：安装项目到本地仓库
	<span style="color:#FF6B6B">注：在<span style="color:red">同一套</span>生命周期中，当运行后面的阶段时，前面的阶段都会运行</span>
	- 执行指定生命周期的两种方式：
		- 在idea中，右侧的maven工具栏，选中对应的生命周期，双击执行
		- 在命令行中，通过命令执行（如：mvn clean）

## 7.测试：

- 作用：用来促进鉴定软件的正确性、完整性、安全性和质量的过程
- 阶段划分：
	- 单元测试：
		- 介绍：对软件的基本组成单位进行测试，最小测试单位
		- 目的：检验软件基本组成单位的正确性
		- 测试人员：开发人员
	- 集成测试：
		- 介绍：将已分别通过测试的单元，按设计要求组合成系统或子系统，再进行的测试
		- 目的：检查单元之间的协作是否正确
		- 测试人员：开发人员
	- 系统测试：
		- 介绍：对已经集成好的软件系统进行彻底的测试
		- 目的：验证软件系统的正确性、性能是否满足指定的要求
		- 测试人员：测试人员
	- 验收测试：
		- 介绍：交付测试，是针对用户需求、业务流程进行的正式测试
		- 目的：验证软件系统是否满足验收标准
		- 测试人员：客户/需求方
- 测试方法：
	- 白盒测试：
		- 清楚软件内部结构、代码逻辑
		- 用于验证代码、逻辑正确性
	- 黑盒测试：
		- 不清楚软件内部结构、代码逻辑
		- 用于验证软件功能、兼容性等方面
	- 灰盒测试：
		- 结合了白盒测试和黑盒测试的特点，既关注软件的内部结构又考虑外部表现（功能）

## 8.单元测试：

- 介绍：针对最小的功能单元（方法），编写测试代码对其正确性进行测试
- JUnit：最流行的Java测试框架之一，提供了一些功能，方便程序进行单元测试
- main方法测试：
	- 测试代码与源代码未分开，难维护
	- 一个方法测试失败，影响后面方法
	- 无法自动化测试，得到测试报告
- JUnit单元测试：
	- 测试代码与源代码分开，便于维护
	- 可根据需要进行自动化测试
	- 可自动分析测试结果，产出测试报告
<span style="color:#FF6B6B">注：JUnit单元测试类名命名规范为：XxxxxTest【规范】；JUnit单元测试的方法必须声明为public void【规定】</span>
- 断言：
	- JUnit提供了一些辅助方法，用来帮我们确定被测试的方法是否按照预期的效果正常工作，这种方式称为断言
	- 单元测试方法运行不报错，不代表业务方法没问题
	- 通过断言可以检测方法运行结果是否和预期一致，从而判断业务方法的正确性
{% asset_img 2.jpg %}
<span style="color:#FF6B6B">注：上述方法形参中的最后一个参数msg表示错误提示信息，可以不指定（有对应的重载方法）</span>
- 常见注解：
{% asset_img 3.jpg %}
<span style="color:#FF6B6B">注：通过@ParameterizedTest进行参数化测试时，需要@ValueSource进行参数设置，并且在单元测试方法上声明方法形参。如下所示：</span>
{% asset_img 4.jpg %}
- 企业开发规范：
	- 原则：编写测试方法时，要尽可能的覆盖业务方法中所有可能的情况（尤其是边界值）
- 依赖范围：
	- 依赖的jar包默认情况下可以在任何地方使用。可以通过< scope >...< /scope >设置其作用范围
	{% asset_img 5.jpg %}
	- 作用范围：
		- 主程序范围有效（main文件夹范围内）
		- 测试程序范围有效（test文件夹范围内）
		- 是否参与打包运行（package指令范围内）
	- 常见取值：
		- compile（默认）：主程序、测试程序、打包（运行）都有效
		- test：仅测试有效
		- provided：主程序、测试程序有效
		- runtime：测试程序、打包（运行）有效