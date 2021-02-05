---
typora-copy-images-to: img
---

# Java 学习笔记

### Installation

* Tutorial [使用IntelliJ IDEA 配置JDK（入门）_nobb111的博客-CSDN博客_idea配置jdk](https://blog.csdn.net/nobb111/article/details/77116259)

* windows环境变量 [Installation of the JDK on Microsoft Windows Platforms (oracle.com)](https://docs.oracle.com/en/java/javase/15/install/installation-jdk-microsoft-windows-platforms.html#GUID-96EB3876-8C7A-4A25-9F3A-A2983FEC016A)

  * 刷新环境变量

  ```bash
  # 管理员cmd (not powershell)
  set PATH=C:
  exit
  # 重新进入cmd
  echo %PATH%
  ```

### IntelliJ Tutorial

[IntelliJ IDEA overview—IntelliJ IDEA (jetbrains.com)](https://www.jetbrains.com/help/idea/discover-intellij-idea.html)

###### JAR

[Create your first Java application—IntelliJ IDEA (jetbrains.com)](https://www.jetbrains.com/help/idea/creating-and-running-your-first-java-application.html#package)

###### 代理

需要使用`Manual proxy configuration`: [Intellij IDEA设置HTTP Proxy - 爱生活的阿琦 - 博客园 (cnblogs.com)](https://www.cnblogs.com/uniquezhangqi/p/9199281.html)

###### 快捷键

如果不能使用，请查看是否有其它软件占用了快捷键

* `Double Shift` to Search Everywhere
  * `Ctrl + N` to Search Class
* `Ctrl + E` to view Recent Files
  * `Ctrl + Shift + E` to view Recent Snippets (locations)
* `Ctrl + F12` or `Alt + 7` to view File Structure
* `Alt + F12` to toggle Terminal
* `Ctrl + Alt + Shift + T` to Refactor
* `Alt + Enter` to Quick Fix
* Code Generation
* Debugger
* Profiler
* `Ctrl + Alt + S` to open Settings
* `Alt + `` to view Version Control
* `Ctrl + J` to view Recommended Templates

###### Javadoc

[Javadocs—IntelliJ IDEA (jetbrains.com)](https://www.jetbrains.com/help/idea/working-with-code-documentation.html)

###### TODO & FIXME

[TODO comments—IntelliJ IDEA (jetbrains.com)](https://www.jetbrains.com/help/idea/using-todo.html)

### Code Convention

[Code Conventions for the Java Programming Language: Contents (oracle.com)](https://www.oracle.com/java/technologies/javase/codeconventions-contents.html)

* Any file exceeds `2000` lines is cumbersome and should be avoided

* The public class (or interface) should be the first class (or interface) of the source file

* class variables should come before methods; static variables should come before non-static (instances)

* Naming

  * Camel Case for `Class`, `method` and `variable`
    * `Class` should be noun, and start with capital letter, e.g. `CalenderDialogView`
    * `method` should be verb, and start with lowercase letter, e.g. `getBrakeSystemType`
  * `CONSTANT` should all be capital letters separated by `_`, which should be a `static final` field within a class

* Wrapping lines

  * Break after a comma
  * Break before an operator
  * Indention
    * the same level with the first line, or 8-space rule
      * `if` statement should generally follow the 8-space rule

* An example of `for` statements, notice the space after `for`

  ```java
  for (initialization; condition; update) {
      statements;
  }
  ```

* An example of `try-catch-finally` statements, notice that `catch` and `finally` are place at the same line of the close-bracket

  ```java
  try {
      statements;
  } catch (ExceptionClass e) {
      statements;
  } finally {
      statements;
  }
  ```

  similar to `if-else` statements

  ```java
  if (condition) {
      statements;
  } else {
      statements;
  }
  ```

* Cast may be followed by a space

  ```java
  myMethod((byte) aNum, (Object) x);
  ```

  

### 相较于C++，Java的特点

1. 单根继承结构

2. heap and stack

* stack（堆栈）: 静态存储区；编译时决定，提高运行时效率
* heap（堆）: 动态存储区；Java除基本类型外，均为动态创建

3. 垃圾回收器

不可以隐藏(同名)变量，以下做法是错误的

```java
{
	int x = 12;
	{
		int x = 96; // Illegal
	}
}
```

#### 基本成员默认值

* 若类的某个成员是基本数据类型，即使没有进行初始化，Java也会确保它获得一个默认值
* 这一机制不适用于“局部”变量，即某个方法中定义的基本类型

### Minor Notification

###### 整数除法直接去除小数部分

###### `float`或`double`转型为整型时，总是对该数字执行截尾

* 四舍五入，请使用`java.lang.Math`中的`round()`方法

###### Random

* `java.util.Random`
* `Random(int seed)`
* `Random.nextInt(int bound)` gives `[0, bound)`
* `Random.nextFloat()` gives `[0.0, 1.0]`

###### 指数计数法默认被处理为`double`类型

###### Java不会自动将`int`转换为`boolean`

`Java`没有`sizeof`因为所有数据类型在所有机器中的大小是相同的

###### `break` and `continue` 与 标签`label`

`Java`要求label恰好置于循环语句的前一行

```java
outer:
while (true) {
	while (true) {
		// do something...
		if (condition) {
			break outer;
		}
	}
}
// continue to here after executing `break outer`
```



---

### 继承覆盖

当子类覆盖父类的域（成员），则当调用子类方法时，使用子类成员；当调用父类方法时，使用父类成员。

* 因为每个类使用成员时都相当于在该成员前加了this指针，而this指针与调用的父类或子类方法所在类一致。

### 尽量不要在构造器内调用非private/final方法，否则继承+覆盖后可能产生错误行为

### 多态

##### 向上转型后动态绑定子类

* 但这并不说明一定调用子类
  * 对于域而言，因为编译器绑定，与定义类型有关（即基类）

