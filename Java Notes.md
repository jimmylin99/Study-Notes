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

```java
@param
@return
{@code}
{@link}
```

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


---

### 第1~4章

#### 相较于C++，Java的特点

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

###### 基本成员默认值

* 若类的某个成员是基本数据类型，即使没有进行初始化，Java也会确保它获得一个默认值
* 这一机制不适用于“局部”变量，即某个方法中定义的基本类型

#### Minor Notification

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

###### 运算符优先级

最高优先级：`()`, `[]`, `.`

高于 `Unary type cast`，即`(type)`

---

### 第5章：初始化与清理

###### `this`关键字

表示调用方法的那个对象

```java
public class Leaf {
	int i = 0;
	Leaf increment() {
		i++;
		return this;
	}
	
	void print() {
		System.out.println("i = " + i);
	}
	
	public static void main(String[] args) {
		Leaf x = new Leaf();
		x.increment().increment().increment().print();
	}
}
```

###### `finalize` 方法

当垃圾回收发生时，被回收对象的`finalize`方法会被调用

* 但是，不仅垃圾回收这件事是否发生不能确定，而且即使发生了垃圾回收，它会释放对象占据的所有内存，所以finalize的用途被限制到了一种特殊情况，即通过某种创建对象方式之外的方式为对象分配了存储空间
  * 主要通过Java调用非Java的本地方法时使用
  * 但在Effective Java中指出，终结函数(finalize)是多余的、危险的，因为你无法预料它（何时被调用）

###### 垃圾回收器如何工作

为了让堆分配变得高效，垃圾回收器时不时会将堆所在的不连续的内存空间重新分配至连续

* 停止-复制(stop-and-copy)机制：暂停程序运行，然后扫描堆栈（stack）或静态存储区的所有引用，将“活”的对象拷贝到新的内存空间
* 标记-清扫(mark-and-sweep)机制：不暂停程序，逐渐扫描和标记“活”的对象；在全部标记动作完成后，开始将未标记的对象清理；这样省略了复制过程，但代价是获得了非连续的内存空间

* 自适应技术，在上述两者间切换；同时引入了“代数”的概念

###### 成员初始化

（类的）域会被自动初始化（先于构造函数），内存中全部被赋予0

方法的局部变量必须初始化后方可使用，否则编译器报错

对于类成员变量，可以在定义的位置直接初始化（C++不允许这样做）

static关键字不能应用于局部变量

###### （成员）初始化顺序

（类的）域的静态对象（如果它们尚未因前面的对象创建过程而被初始化）> （类的）域的non-static对象 > 构造函数（方法）

* 如果以对象（类）访问静态域，则不调用non-static对象的初始化和构造函数！
  * 只要访问一个静态域，其余所有静态域都会被初始化

* 静态域一经被初始化，不再被重复初始化

###### 静态子句

```java
public class Spoon {
	static int i;
	static {
		i = 47;
	}
}
```

等价于静态初始化

###### 非静态初始化子句

与静态子句相比只去掉static关键字，对non-static域有效。这个就比较有用，它可以保证无论调用哪个构造函数，子句内的域都可以被初始化，且初始化发生在构造函数之前

###### 数组

```java
int[] a;
int a[];
int[] a = {1, 2, 3};
int[] a = new int[5];
Integer[] a = {
    new Integer(1),
    new Integer(2)
}	// java.util.Integer

```

###### 可变参数

###### 枚举类型

Treated similar to classical Class

```java
public enum ClassName {
	HOT, MILD, COLD
}
```

### 第6章：访问权限控制

###### `import static` 引入静态方法

###### 包访问权限

###### protected也提供包访问权限

###### 至多一个public类，且该public类名称与文件名一致

* 意味着可以没有public类，且类名可与文件名不一致

###### Singleton(单例)

```java
class Singleton {
    private static final Singleton singleton = new Singleton();
    private Singleton() {}
    public static Singleton access() {
        return singleton;
    }
    public void f() {}
}

class TestSingleton {
    public static void main(String[] args) {
        Singleton s = Singleton.access();
        s.f();
    }
}
```

### 第7章：复用类

###### 基类构造器总是会被调用，且在导出类构造器之前被调用

###### 调用基类构造器必须是在导出类构造器中要做的第一件事

###### 代理

它可以用来替换继承，同时移除部分接口

* IDEA中右键类，Refactor

###### 清理顺序

一般而言，应该与构造顺序相反，即先执行类的所有清理动作，然后再调用基类的清理动作

###### `@Override`

注明要覆盖基类方法，用来避免误引入预期之外的方法

###### 避免protected域

尽量将域保持为private，转而使用protected方法来控制继承者的访问权限

* 应当一直保留“更改底层实现”这一权利

###### Blank final

没有在定义时初始化的final域，必须要在构造器内初始化

* 这提供了更多的灵活性

###### final参数、final类

###### final方法保证在继承中不会被覆盖

### 第8章：多态

###### 继承覆盖

当子类覆盖父类的域（成员），则当调用子类方法时，使用子类成员；当调用父类方法时，使用父类成员。

* 因为每个类使用成员时都相当于在该成员前加了this指针，而this指针与调用的父类或子类方法所在类一致。

###### 尽量不要在构造器内调用未指定为private/final的方法，否则继承+覆盖后可能产生错误行为

* 因为调用的方法若已被覆盖，则以覆盖的为准
* 尽量只在构造器中调用基类的final方法

###### 向上转型后动态绑定子类

* 但这并不说明一定调用子类
  * 对于域而言，因为编译器绑定，与定义类型有关（即基类）

### 第9章：接口

###### 抽象方法、抽象类

###### 接口的域是`static+private`

###### 接口的方法是`public`

###### 只能至多继承一个类，但可以实现多个接口

###### 优先选择类而不是接口

* 如果接口的必须行变得非常明确，那么就进行重构

### 第10章：内部类

###### non-static内部类有外部类的所有权限

* 因为内部类会获得调用它的外部类的引用

###### this

内部类中`this`指代内部类的引用

若要获取外部类引用，请使用`OutClassName.this`

###### 创建内部类的引用

外部类的non-static方法中，正常创建

static方法中

```java
OuterClass out = new OuterClass();
OuterClass.Inner inn = out.new OuterClass.InnerClass();
```

###### 匿名内部类

```java
class CertainClass {
	public OneClass methodName(methodParameter) {
		// ...
		return new OneClass(parameter) {
			// anonymous class definition 
			// ...
		}
	}
}
```

###### 内部类可以解决多重继承问题

```java
class D() {}
abstract class E() {}
class Z extends D {
	E makeE() {
		return new E() {
			//...
		}
	}
}
```

###### 闭包与回调

### 第11章：持有对象



### 第12章：异常处理

* `System.err` 与 `System.out` 输出的相对顺序似乎不能被保证

###### 异常说明

除了`Error`及其子类（`Error` is the direct subclass of `Throwable`）和`RuntimeException`及其子类 (`RuntimeException` is the direct subclass of `Exception`)，其余在构造器或方法内抛出的异常，均需要在头部定义异常说明

```java
void f() throws TooBig, TooSmall, DivZero {
	// ...
    throw new TooBig();
    // ...
}
```

###### `finally`

`finally` guanratees that statements within it will always be executed regardless of whether there is an exception thrown from `try` block

* Usually used for disposing resources manually

Even if `return` is executed within `try` or `catch` block, the `finally` block will always be executed

###### 异常丢失

1. A new exception is thrown before the previous exception being handled, then the new one will take in place of the latter one, while the latter one becomes lost
   1. An exception arisen from within `finally` block may result in such case
2. Return from within `finally` block may lose exception

###### 异常的限制

General Rule: In most cases, when the method is overridden, it can only throw exceptions listed in the `throws` clause of the base class.

* Constructor can implement other exceptions in addition to those thrown from base class
* Methods in derived class do not necessarily define exceptions in `throws` clause as base class does

TODO: Why do we need this limitation? The explanation given in the book is that the codes, including exceptions, for base class still work in the derived classes (它允许派生类自由替换基类)

###### 打印栈轨迹

Remember to print stack trace within `catch` clause when you are not prepared to deal with exceptions but forced to add `catch` clause

* This prevents from swallowing exceptions

```java
try {
	// ...
} catch (Exception e) {
	e.printStackTrace(System.out);
}
```

### 第14章：类型信息

每个类都有一个Class对象（编译时生成），每个类的（常规）创建是由这个类的Class对象完成的，

###### 类加载器

JVM通过类加载器来动态加载类，首先判断类的Class对象是否被加载，加载后再加载需要的部分

###### 类字面常量

类字面常量返回对该类Class对象的引用

`FancyToy.class` 相比于 `Class.forName("FancyToy")` 更安全，因为类字面常量会在编译时受到检查

它可以被应用于普通的类、接口、数组以及基本数据类型

* 包装类中的`TYPE`字段是基本数据类型`.class`类字面常量的引用

