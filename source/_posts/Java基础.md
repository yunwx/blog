---
title: Java基础
date: 2024-11-29 18:54:19
tags: Java
categories: Java
---

## 一、入门

#### 1.Java核心优势：跨平台

#### 2.运行机制：

- 源文件（java文件）->编译器->字节码（class文件）->JVM虚拟机->操作系统

#### 3.Java语言即有编译型也有解释型

#### 4.main方法：

- Java应用程序的入口方法，格式固定：pulic static void main(String[] args){}

#### 5.每个类都会生成一个class文件

#### 6.JDK和JRE和JVM：
{% asset_img 1.jpg %}
####  7.DOS命令：

- cd 目录路径：进入目录
- cd..：进入父目录
- dir：查看本目录下的文件和子目录列表
- cls：清除屏幕命令

#### 8.十进制转二进制：除2取余，逆序排列

#### 9.注释：

- 单行注释：//
- 多行注释：/*   */
- 文档注释：/**  */

#### 10.Java不采用ASCII字符集，采用Unicode字符集

#### 11.Java方法中所有参数都是“值传递”

## 二、变量

#### 1.变量：局部变量、成员变量、静态变量

<span style="color:#FF6B6B">注：成员变量属于对象，静态变量属于类</span>

#### 2.数据类型：

- 数值型：
	- 整型：byte（1字节）、short（2字节）、int（4字节）、long（8字节）
	- 浮点型：float（4字节）、double（8字节）
- 字符型：char
- 布尔型：boolean（未赋值则初始化为false）
 
<span style="color:#FF6B6B">注：字节：byte，1byte=8bit</span>

#### 3.二进制以0(零)b开头

#### 4.默认类型：

- 整型变量默认类型是int，在数字后加L可以把整型常量定义为long类型
- 浮点常量默认类型是double，在数字后加F可以把整型常量定义为float类型

#### 5.取余：余数符号与左边操作数相同

- 例：-7%3=-1，7%-3=1

#### 6.逻辑运算符：

- ＆（与）：一个为false，结果则为false
- ｜（或）：一个为true，结果则为true
-  ！（非）：取反（true为false，false为true）
-  ^（异或）：相同为false，不同为true
- ＆＆（短路与）：前面为false，后面则不计算
- ｜｜（短路或）：前面为true，后面则不计算

#### 7."+"（字符串连接符）：

- 条件必须是String，不能是char（若是char仍是加法）
- 例：

``` java
		char c1='h';
        char c2='i';
        System.out.println(c1+c2);//输出为数字
        System.out.println(""+c1+c2);//输出为hi)
```

#### 8.优先级：＆比｜高，＆＆比｜｜高

#### 9.自动类型转换（隐式类型转换）：

**自动类型转换规则：**

(1)数值型（整型 → 整型 / 整型 → 浮点型）
- byte → short → int → long → float → double
- char（2字节）可以自动转 int（4字节）或更高
  
(2)char的特殊情况
- char可以自动转 int（因为 char 是无符号 16 位，int 是 32 位）
- 但byte/short 不能自动转 char（可能超出范围）
  
**计算时的自动类型转换：**

(1) 整型运算（至少提升到int）
- byte、short、char 在计算时会先转int
- 如果有long/float/double，则向更大的类型转换
  
(2) 混合运算（整型 + 浮点型 → 浮点型）

#### 10.强制转换(Type)

- 例：

``` java
(int)a; //把a强制转换为int类型
```

#### 11.数据溢出：

**定义：** 指的是当变量的值 超出其数据类型的存储范围时，导致数据丢失或异常结果的情况。数据溢出通常发生在 整型（int、long、byte、short）运算中，但浮点型（float、double）也可能出现精度问题。

**整型数据溢出:**

(1) 什么是整型溢出？
- 当计算结果超出数据类型的最大值或最小值 时，数值会 "回绕"（wrap around）：
- 超过最大值→变成最小值
- 低于最小值→变成最大值
  
(2) 常见溢出场景
{% asset_img 2.jpg %}

**浮点型的精度问题:**

(1)虽然float和double不会像整型那样 "回绕"，但浮点数运算可能存在精度丢失：
- float（32位）：有效位数约 6~7 位
- double（64位）：有效位数约 15~16 位
  
**如何避免数据溢出？**
(1) 使用更大的数据类型
(2) 检查运算是否溢出:Java 8+ 提供了 Math.addExact(), Math.multiplyExact() 等方法，溢出时会抛出 ArithmeticException
(3) 使用BigInteger（超大整数）或 BigDecimal（高精度小数）

## 三、控制语句

#### 1.条件语句

(1) if-else 语句

``` java
if (条件1) {
    // 条件1为 true 时执行
} else if (条件2) {
    // 条件2为 true 时执行
} else {
    // 所有条件都不满足时执行
}
```

(2) switch-case 语句

``` java
switch (变量) {
    case 值1:
        // 匹配值1时执行
        break;
    case 值2:
        // 匹配值2时执行
        break;
    default:
        // 所有 case 都不匹配时执行
}
```
<span style="color:#FF6B6B">注：</span>
- <span style="color:#FF6B6B">switch支持int、char、String(Java 7+)、enum(枚举)</span>
- <span style="color:#FF6B6B">不加break会"穿透"（fall-through） 执行后续 case</span>

#### 2. 循环语句

(1) for 循环

``` java
for (初始化; 条件; 更新) {
    // 循环体
}
```

(2) while 循环

``` java
while (条件) {
    // 循环体
}
```

(3) do-while 循环

``` java
do {
    // 循环体（至少执行一次）
} while (条件);
```

(4) 增强 for 循环（for-each）

``` java
for (数据类型 变量 : 数组/集合) {
    // 循环体
}
```

#### 3. 跳转语句

(1) break
- 立即终止当前循环或 switch 语句

``` java
for (int i = 0; i < 10; i++) {
    if (i == 5) {
        break;  // 当 i=5 时跳出循环
    }
    System.out.println(i);  // 输出 0,1,2,3,4
}
```

(2) continue
- 跳过本次循环，进入下一次循环

``` java
for (int i = 0; i < 5; i++) {
    if (i == 2) {
        continue;  // 跳过 i=2 的情况
    }
    System.out.println(i);  // 输出 0,1,3,4
}
```

(3) return
- 退出当前方法，并返回一个值（如果有返回值）

``` java
public int add(int a, int b) {
    return a + b;  // 返回 a + b 的结果
}
```

(4) 带标签的 break 和 continue
- 可以跳出多层循环（适用于嵌套循环）

``` java
outerLoop:  // 标签
for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 3; j++) {
        if (i == 1 && j == 1) {
            break outerLoop;  // 直接跳出外层循环
        }
        System.out.println(i + "," + j);
    }
}
```

## 四、构造器

#### 1.构造器通过new关键字调用

#### 2.返回值：

- 构造器虽然有返回值，但不能定义返回值类型（返回值的类型肯定是本类）
- 不能在构造器里使用return返回值

#### 3.没有定义构造器：

- 若没有定义构造器，编译器会自动定义一个无参构造方法，若定义了则不会添加

#### 4.构造器方法名必须和类名一致

#### 5.构造器调用构造器:

- 可以用this()，但必须位于第一行
	- this（隐式参数）代表当前对象本身（对象地址）
	- this不能在static方法中使用（因为static属于类）

## 五、常用修饰符

#### 1. static 修饰符

(1) 作用：表示类级别的成员（与对象无关），所有对象共享同一份数据。
(2) 应用场景：
- 修饰变量：静态变量（类变量），如 static int count = 0;
- 修饰方法：静态方法（类方法），如 static void print() { ... }
- 修饰代码块：静态代码块（类加载时执行），如 static { ... }
- 修饰嵌套类：静态内部类，如 static class Inner { ... }
  
(3) 特点：
- 静态方法不能访问非静态成员（因为不依赖对象）。
- 静态代码块在类加载时执行一次（用于初始化静态变量）。
- 静态变量存储在方法区，所有对象共享。

#### 2. final 修饰符

(1) 作用：表示不可变，用于限制变量、方法或类的修改。
(2) 应用场景：
- 修饰变量：常量（值不可变），如 final int MAX = 100;
- 修饰方法：禁止子类重写，如 final void show() { ... }
- 修饰类：禁止继承，如 final class MathUtils { ... }
  
(3) 特点：
- final变量：
	- 基本类型：值不可变。
	- 引用类型：引用不可变（但对象内部状态可修改）。
- final方法：子类不能重写（但可以重载）。
- final类：不能被继承（如 String、Integer）。

#### 3. abstract 修饰符

- 修饰类：不能实例化，需子类继承。
- 修饰方法：只有声明，无实现（需子类重写）。

#### 4. synchronized 修饰符

- 用于方法或代码块，保证线程安全。

#### 5. volatile 修饰符

- 保证变量的可见性（多线程场景）。

#### 6. native 修饰符

- 表示方法由本地代码（如 C/C++）实现。

## 六、面向对象

#### 1.面向过程与面向对象：

- 面向过程是“执行者思维”
- 面向对象是“设计者思维”
- 面向对象离不开面向过程：
	- 宏观上，通过面向对象进行整体设计
	- 微观上，执行和处理数据仍然是面向过程

#### 2. 类和对象（Class & Object）

(1) 类（Class）
- 类是对象的模板，定义对象的属性（成员变量）和行为（方法）。
- 语法：

``` java
public class 类名 {
    // 成员变量（属性）
    数据类型 变量名;

    // 方法（行为）
    返回类型 方法名(参数列表) {
        // 方法体
    }
}
```

(2) 对象（Object）
- 对象是类的实例，通过new关键字创建。
- 语法：

``` java
类名 对象名 = new 类名();
```

#### 3. 封装

**定义：** 指隐藏对象的内部细节，仅对外提供访问接口（getter/setter）。

(1) 访问修饰符
- private：仅当前类可访问
- protected：当前类+子类+同包类可访问
- public：所有类可访问
- default(缺省)：当前类+同包类可访问
  
(2) Getter/Setter
- 示例：

``` java
public class Person {
    private String name;  // 私有属性

    // Getter
    public String getName() {
        return name;
    }

    // Setter
    public void setName(String name) {
        this.name = name;  // this 指当前对象
    }
}
```

- 使用：

``` java
Person p = new Person();
p.setName("Bob");  // 通过 setter 修改 name
System.out.println(p.getName());  // 通过 getter 获取 name
```

#### 4.继承

**定义：** 允许子类复用父类的属性和方法，并可以扩展新功能。

(1) 语法

``` java
class 子类 extends 父类 {
    // 新增属性或方法
}
```

- 示例：

``` java
class Animal {
    void eat() {
        System.out.println("Animal is eating");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("Dog is barking");
    }
}
```

- 使用：

``` java
Dog dog = new Dog();
dog.eat();  // 继承自 Animal
dog.bark(); // Dog 新增方法
```

(2) 方法重写（Override）
- 子类可以重写父类的方法：

``` java
class Dog extends Animal {
    @Override  // 注解，表示重写（可选）
    void eat() {
        System.out.println("Dog is eating");
    }
}
```

(3) super 关键字
- 用于调用父类的构造方法或成员方法（访问父类被子类覆盖的方法或属性）：

``` java
class Dog extends Animal {
    Dog() {
        super();  // 调用父类构造方法
    }

    @Override
    void eat() {
        super.eat();  // 先调用父类的 eat()
        System.out.println("Dog is eating");
    }
}
```

<span style="color:#FF6B6B">注：</span>
- <span style="color:#FF6B6B">一个类若构造方法第一行没调用super(...)或this(...)，那么Java默认调用super()，即调用父类的无参构造方法</span>
- <span style="color:#FF6B6B">构造方法调用顺序：先向上追溯到Object类，然后再依次向下执行类的初始化块和构造方法，直到子类为止</span>
- <span style="color:#FF6B6B">属性/方法查找：查找当前类→依次向上追溯每个父类，直到Object类→没找到则报错</span>

#### 5. 多态

**定义：** 指同一个方法在不同情况下有不同的行为。

(1) 方法重载
- 同一个类中，方法名相同，参数不同：

``` java
class Calculator {
    int add(int a, int b) {
        return a + b;
    }

    double add(double a, double b) {
        return a + b;
    }
}
```

<span style="color:#FF6B6B">注：</span>
- <span style="color:#FF6B6B">重载是形参不同（形参类型，形参个数，形参顺序），名称相同，实际上是完全不同的方法</span>
- <span style="color:#FF6B6B">只有返回值类型不同不构成重载（编译器会报错）</span>
- <span style="color:#FF6B6B">形参只是名称不同也不构成重载</span>

(2) 向上转型
- 父类引用指向子类对象：

``` java
Animal animal = new Dog();  // Dog 是 Animal 的子类
animal.eat();  // 调用的是 Dog 的 eat()
```

(3) instanceof 判断类型
- 判断左边的对象是否属于右边的类：

``` java
if (animal instanceof Dog) {
    Dog dog = (Dog) animal;  // 向下转型
    dog.bark();
}
```

#### 6. 抽象类

- 用abstract声明，不能直接实例化，只能被继承。
- 可以包含抽象方法（无方法体）和普通方法。

``` java
abstract class Shape {
    abstract void draw();  // 抽象方法

    void print() {
        System.out.println("This is a shape");
    }
}

class Circle extends Shape {
    @Override
    void draw() {
        System.out.println("Drawing a circle");
    }
}
```

#### 7. 接口

- 用interface声明，所有方法默认public abstract。
- Java 8+ 支持默认方法（default）和静态方法（static）。

``` java
interface Flyable {
    void fly();  // 抽象方法

    default void land() {  // 默认方法
        System.out.println("Landing...");
    }
}

class Bird implements Flyable {
    @Override
    public void fly() {
        System.out.println("Bird is flying");
    }
}
```
