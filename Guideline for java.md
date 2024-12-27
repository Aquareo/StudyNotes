# StudyNotes
This is a programming learning record

# 1.访问控制修饰符`protected`、`private` 和 `public`

在Java中，`protected`、`private` 和 `public` 是三种不同的访问控制修饰符，它们控制了类、方法或成员变量的可见性和访问权限。

## private

- 只能在当前类中访问。
- 无法在其他类或子类中访问，即使是同一个包中的类也无法访问。
- 主要用于封装数据，保护类内部的实现细节。

## protected

- 可以在当前类、同一个包中的类，以及当前类的子类中访问。
- 如果子类在不同的包中，protected成员依然是可访问的。

## public

- 可以被任何类访问，无论是在同一个包中还是不同包中。
- 公开的访问权限，适用于那些需要外部访问的成员。

### 总结：

- `private`: 只能在本类中访问。
- `protected`: 可以在本类、同包类和子类中访问。
- `public`: 可以在任何地方访问。

## 继承与访问控制

继承有个特点，就是子类无法访问父类的 `private` 字段或者 `private` 方法。例如，`Student` 类就无法访问 `Person` 类的 `name` 和 `age` 字段：

```java
class Person {
    private String name;
    private int age;
}

class Student extends Person {
    public String hello() {
        return "Hello, " + name; // 编译错误：无法访问name字段
    }
}
```



这使得继承的作用被削弱了。为了让子类可以访问父类的字段，我们需要把`private`改为`protected`。用`protected`修饰的字段可以被子类访问：

```java
class Person {
    protected String name;
    protected int age;
}

class Student extends Person {
    public String hello() {
        return "Hello, " + name; // OK!
    }
}
```
# 2.多态
在 Java 中，多态允许同一方法调用表现出不同的行为。在本例中，我们将演示如何通过多态实现打印不同类型文档的功能。

### Java多态例子：实现打印机多态
```java
abstract class Document {
    abstract void print();
}

class TextDocument extends Document {
    @Override
    void print() {
        System.out.println("Printing text document...");
    }
}

class PdfDocument extends Document {
    @Override
    void print() {
        System.out.println("Printing PDF document...");
    }
}

class ImageDocument extends Document {
    @Override
    void print() {
        System.out.println("Printing image document...");
    }
}

public class PrinterApp {
    public static void printDocument(Document document) {
        document.print();  // 调用多态方法
    }

    public static void main(String[] args) {
        Document textDoc = new TextDocument();
        Document pdfDoc = new PdfDocument();
        Document imageDoc = new ImageDocument();

        printDocument(textDoc);   // 输出：Printing text document...
        printDocument(pdfDoc);    // 输出：Printing PDF document...
        printDocument(imageDoc);  // 输出：Printing image document...
    }
}

```
由于多态的存在，每个子类都可以覆写父类的方法，例如：

```java
class Person {
    public void run() { … }
}

class Student extends Person {
    @Override
    public void run() { … }
}

class Teacher extends Person {
    @Override
    public void run() { … }
}
```


如果父类`Person`的`run()`方法没有实际意义，能否去掉方法的执行语句？不行

```java

class Person {
    public void run(); // Compile Error!
}
```
能不能去掉父类的`run()`方法？
答案还是不行，因为去掉父类的`run()`方法，就失去了多态的特性。例如，runTwice()就无法编译：

```java
public void runTwice(Person p) {
    p.run(); // Person没有run()方法，会导致编译错误
    p.run();
}

```

如果父类的方法本身不需要实现任何功能，仅仅是为了定义方法签名，目的是让子类去覆写它，那么，可以把父类的方法声明为抽象方法：

```java
class Person {
    public abstract void run();
}

```


把一个方法声明为abstract，表示它是一个抽象方法，本身没有实现任何方法语句。因为这个抽象方法本身是无法执行的，所以，Person类也无法被实例化。编译器会告诉我们，无法编译Person类，因为它包含抽象方法。

必须把Person类本身也声明为abstract，才能正确编译它：

```java
abstract class Person {
    public abstract void run();
}

```

使用abstract修饰的类就是抽象类。我们无法实例化一个抽象类：

```
Person p = new Person(); // 编译错误
```

无法实例化的抽象类有什么用？

因为抽象类本身被设计成只能用于被继承，因此，抽象类可以强迫子类实现其定义的抽象方法，否则编译会报错。因此，抽象方法实际上相当于定义了“规范”。



如果一个抽象类没有字段，所有方法全部都是抽象方法，就可以把该抽象类改写为接口：`interface`。

在Java中，使用`interface`可以声明一个接口：

```java
interface Person {
    void run();
    String getName();
}
```

# 3.Static

## 实例方法与静态方法的区别

在 Java 中，方法可以分为两类：实例方法和静态方法。这两种方法的使用方式和它们与类的关系有所不同。

### 实例方法 (Non-static Method)
实例方法是属于类的实例对象的，它必须通过创建类的实例后，才能调用。

#### 关键特点：
- **属于对象**：实例方法依赖于对象的状态。
- **需要实例化对象**：必须通过对象来调用实例方法。
  
#### 示例：
```java
class A {
    public String b() {
        return "123";
    }
}

public class Main {
    public static void main(String[] args) {
        A obj = new A();  // 创建 A 类的对象
        System.out.println(obj.b());  // 通过对象调用实例方法
    }
}
```
在上述代码中，`b()` 是一个实例方法，它需要通过 `A` 类的对象 `obj` 来调用。


### 静态方法 (Static Method)

```java

class A {
    public static String b() {
        return "123";
    }
}

public class Main {
    public static void main(String[] args) {
        System.out.println(A.b());  // 直接通过类名调用静态方法
    }
}

```
### 总结
当在方法 `b()` 前面加上 `static` 修饰符时，它就变成了一个 静态方法。静态方法属于类本身，而不是类的某个实例对象。因此，可以直接通过类名来调用静态方法，而不需要创建类的实例。



# 4.多态回顾

## 关于父类和子类的转换

```java
// 定义父类
abstract class makelove {
    public abstract void fuck();
}

// 定义子类 footjob，继承自 makelove
class footjob extends makelove {
    @Override
    public void fuck() {
        System.out.println("bbc");
    }

    // footjob 类独有的方法
    public void specialMethod() {
        System.out.println("This is a special method.");
    }
}

public class Main {
    public static void main(String[] args) {
        // 使用父类引用指向子类对象
        makelove a = new footjob();
        a.fuck();  // 输出 "bbc"

        // 如果想调用子类特有的方法，需要强制转换
        if (a instanceof footjob) {
            footjob f = (footjob) a;
            f.specialMethod();  // 输出 "This is a special method."
        }
    }
}
```
###  向上转型（Upcasting）

```java
makelove a = new footjob();
```
其实是一个隐式的向上转型。```footjob``` 是 ```makelove``` 的子类，子类对象本来就包含父类的所有方法和字段。

向上转型是安全的，因为子类对象一定具备父类的结构。即使你只用父类引用，也能访问父类定义的属性和方法。

向上转型的好处是你可以通过父类的引用来操作不同子类的对象，这也是 多态 的体现。

###  访问限制

### 解释

- `a instanceof footjob` 是检查 `a` 是否是 `footjob` 类型的对象。
- 如果条件成立，`a` 会被强制转换为 `footjob` 类型，可以访问 `footjob` 类中的方法 `specialMethod()`。



### 子类覆写了父类的方法

```java
// override
public class Main {
    public static void main(String[] args) {
        Person p = new Student();
        p.run(); // 应该打印Person.run还是Student.run?
    }
}

class Person {
    public void run() {
        System.out.println("Person.run");
    }
}

class Student extends Person {
    @Override
    public void run() {
        System.out.println("Student.run");
    }
}

```


分析，一个实际类型为`Student`，引用类型为`Person`的变量，调用其`run()`方法，调用的是`Person`还是`Student`的`run()`方法？

```java
Person p = new Student();
p.run(); // 无法确定运行时究竟调用哪个run()方法

```


非常奇怪的事情，`p.run()`运行的是`Student`的，但是确运行不了`p.test()`

```java

// override
public class Main {
    public static void main(String[] args) {
        Person p = new Student();
        p.run(); // 应该打印Person.run还是Student.run?
        p.test();// 会报错
    }
}

class Person {
    public void run() {
        System.out.println("Person.run");
    }
}

class Student extends Person {
    @Override
    public void run() {
        System.out.println("Student.run");
    }
    public void test()
    {
        System.out.println("test");
    }
}

```

其实可以这么来理解，`p`的声明类型是`Person`和实际类型是`Student`，声明类型限制了不能调用`Student`里面的`test()`，实际类型确可以使`p`能多态使用`Student`里面的'run()'

## 抽象类 vs 接口


### 接口

在抽象类中，抽象方法本质上是定义接口规范：即规定高层类的接口，从而保证所有子类都有相同的接口实现，这样，多态就能发挥出威力。

如果一个抽象类没有字段，所有方法全部都是抽象方法：

```java
abstract class Person {
    public abstract void run();
    public abstract String getName();
}
```

就可以把该抽象类改写为接口：`interface`。
在Java中，使用`interface`可以声明一个接口:

```java
interface Person {
    void run();
    String getName();
}
```

### 接口继承
```java
interface Hello {
    void hello();
}

interface Person extends Hello {
    void run();
    String getName();
}
```

我们知道，在Java中，一个类只能继承自另一个类，不能从多个类继承。但是，一个类可以实现多个`interface`，例如：

```java
class Student implements Person, Hello { // 实现了两个interface
    ...
}
```

### default方法

```java

interface Person {
    void run();
    String getName();
}
```

在接口中，可以定义`default`方法。例如，把`Person`接口的`run()`方法改为`default`方法：

```java
interface Person {
    String getName();
    default void run() {
        System.out.println(getName() + " run");
    }
}
```

###`default`方法和抽象类的普通方法的区别
`interface`没有字段，`default`方法无法访问字段，而抽象类的普通方法可以访问实例字段。


`default`方法的目的是，当我们需要给接口新增一个方法时，会涉及到修改全部子类。如果新增的是`default`方法，那么子类就不必全部修改，只需要在需要覆写的地方去覆写新增方法。



| 特性         | 抽象类                                   | 接口                                         |
|--------------|------------------------------------------|----------------------------------------------|
| 是否可以实例化 | 不能实例化                                | 不能实例化                                    |
| 是否可以有构造方法 | 可以有构造方法                           | 不能有构造方法                               |
| 方法的实现     | 可以有抽象方法和普通方法（有方法体）      | 默认方法有方法体，抽象方法没有方法体         |
| 字段           | 可以有实例字段和静态字段                  | 只能有常量（`public static final`）           |
| 多重继承       | 一个类只能继承一个抽象类                  | 一个类可以实现多个接口                       |
| 使用场景       | 用于共享代码和定义通用行为                | 用于定义能力或功能的合约                     |
| 访问修饰符     | 可以使用 `public`, `protected`, `private` 等 | 方法和字段默认是 `public`                    |



### 例子

```java
abstract class Animal {
    // 抽象方法，子类必须实现
    public abstract void sound();
    
    // 普通方法，子类可以继承并使用
    public void breathe() {
        System.out.println("Animal is breathing");
    }
}

class Dog extends Animal {
    // 必须实现抽象方法
    public void sound() {
        System.out.println("Bark");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.sound();   // Output: Bark
        dog.breathe(); // Output: Animal is breathing
    }
}

```
    
```java
interface Animal {
    // 抽象方法，必须实现
    void sound();
    
    // 默认方法（Java 8及之后支持）
    default void breathe() {
        System.out.println("Animal is breathing");
    }
}

class Dog implements Animal {
    // 必须实现接口中的抽象方法
    public void sound() {
        System.out.println("Bark");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.sound();   // Output: Bark
        dog.breathe(); // Output: Animal is breathing
    }
}

```


```java
// 定义一个接口 Flyable
interface Flyable {
    void fly();  // 接口中的方法默认是抽象的
}

// 定义一个接口 Swimable
interface Swimable {
    void swim();  // 接口中的方法默认是抽象的
}

// 继承 Flyable 和 Swimable 接口的 Duck 类
class Duck implements Flyable, Swimable {
    @Override
    public void fly() {
        System.out.println("Duck is flying.");
    }

    @Override
    public void swim() {
        System.out.println("Duck is swimming.");
    }
}

// 继承 Flyable 接口的 Bird 类
class Bird implements Flyable {
    @Override
    public void fly() {
        System.out.println("Bird is flying.");
    }
}

public class Main {
    public static void main(String[] args) {
        Duck duck = new Duck();
        duck.fly();  // 调用 fly 方法
        duck.swim();  // 调用 swim 方法

        Bird bird = new Bird();
        bird.fly();  // 调用 fly 方法
    }
}


```



# 5.静态字段和静态方法

# 静态字段

在一个class中定义的字段，我们称之为实例字段。实例字段的特点是，每个实例都有独立的字段，各个实例的同名字段互不影响。

是用`static`修饰的字段，称为静态字段：`static field`

实例字段在每个实例中都有自己的一个独立“空间”，但是静态字段只有一个共享“空间”，所有实例都会共享该字段。举个例子：

```java
class Person {
    public String name;
    public int age;
    // 定义静态字段number:
    public static int number;
}
```

虽然实例可以访问静态字段，但是它们指向的其实都是`Person class`的静态字段。所以，所有实例共享一个静态字段。

# 接口的静态字段
因为`interface`是一个纯抽象类，所以它不能定义实例字段。但是，`interface`是可以有静态字段的，并且静态字段必须为`final`类型：

```java
public interface Person {
    public static final int MALE = 1;
    public static final int FEMALE = 2;
}

```
实际上，因为`interface`的字段只能是`public static final`类型，所以我们可以把这些修饰符都去掉，上述代码可以简写为：

```java
public interface Person {
    // 编译器会自动加上public static final:
    int MALE = 1;
    int FEMALE = 2;
}
```


# 静态方法

```java
public class Test
{
    public static void main(String[] args)
    {
        P.fun();
    }
}

class P
{
    public static void fun()
    {
        System.out.println("Fuck World"); // Changed the message to be more appropriate
    }
}

```

# 6.包

Java定义了一种名字空间，称之为包：`package`。一个类总是属于某个包，类名（比如`Person`）只是一个简写，真正的完整类名是`包名.类名`。



```java
package ming; // 申明包名ming

public class Person {
}
```

## 包作用域

没有指定访问修饰符（如`public`、`protected`、`private`），默认的访问权限就是包作用域。

```java
package hello;

class Person {
    // 包作用域字段
    String name;

    // 包作用域方法
    void sayHello() {
        System.out.println("Hello, " + name);
    }
}

class Employee {
    void introduce() {
        Person person = new Person();
        person.name = "Alice";  // 可以访问Person类的name字段，因为它在同一个包内
        person.sayHello();      // 可以访问Person类的sayHello方法，因为它在同一个包内
    }
}

```

## import

在一个`class`中，我们总会引用其他的`class`。例如，小明的`ming.Person`类，如果要引用小军的`mr.jun.Arrays`类，他有三种写法：

第一种

```java
// Person.java
package ming;

public class Person {
    public void run() {
        // 写完整类名: mr.jun.Arrays
        mr.jun.Arrays arrays = new mr.jun.Arrays();
    }
}

```

第二种写法是用import语句，导入小军的Arrays，然后写简单类名：

```java
// Person.java
package ming;

// 导入完整类名:
import mr.jun.Arrays;

public class Person {
    public void run() {
        // 写简单类名: Arrays
        Arrays arrays = new Arrays();
    }
}

```

第三种，在写`import`的时候，可以使用`*`，表示把这个包下面的所有`class`都导入进来（但不包括子包的`class`）：

```java
// Person.java
package ming;

// 导入mr.jun包的所有class:
import mr.jun.*;

public class Person {
    public void run() {
        Arrays arrays = new Arrays();
    }
}

```
一般不推荐这种写法，因为在导入了多个包后，很难看出`Arrays`类属于哪个包。

java调用`class`的识别顺序

- 如果是完整类名，就直接根据完整类名查找这个`class`；
- 如果是简单类名，按下面的顺序依次查找：
  - 查找当前`package`是否存在这个`class`；
  - 查找`import`的包是否包含这个`class`；
  -  查找`java.lang`包是否包含这个`class`。
 

# 7.内部类



如果一个类定义在另一个类的内部，这个类就是Inner Class：


```java
class Outer {
    class Inner {
        // 定义了一个Inner Class
    }
}

```
它与普通类有个最大的不同，就是Inner Class的实例不能单独存在，必须依附于一个Outer Class的实例。

```java

// inner class
public class Main {
    public static void main(String[] args) {
        Outer outer = new Outer("Nested"); // 实例化一个Outer
        Outer.Inner inner = outer.new Inner(); // 实例化一个Inner
        inner.hello();
    }
}

class Outer {
    private String name;

    Outer(String name) {
        this.name = name;
    }

    class Inner {
        void hello() {
            System.out.println("Hello, " + Outer.this.name);
        }
    }
}

```


观察上述代码，要实例化一个`Inner`，我们必须首先创建一个`Outer`的实例，然后，调用`Outer`实例的`new`来创建`Inner`实例：


```java
uter.Inner inner = outer.new Inner();
```

这是因为Inner Class除了有一个`this`指向它自己，还隐含地持有一个Outer Class实例，可以用`Outer.this`访问这个实例。所以，实例化一个Inner Class不能脱离Outer实例。

Inner Class和普通Class相比，除了能引用Outer实例外，还有一个额外的“特权”，就是可以修改Outer Class的`private`字段


# 8.Java核心类

这一部分讲得是Java的特性，可以慢慢了解

## 字符串

### String

在Java中，`String`是一个引用类型，它本身也是一个`class`。但是，Java编译器对`String`有特殊处理，即可以直接用`"..."`来表示一个字符串：


```java
String s1 = "Hello!";
```
实际上字符串在`String`内部是通过一个`char[]`数组表示的，因此，按下面的写法也是可以的：

```java
String s2 = new String(new char[] {'H', 'e', 'l', 'l', 'o', '!'});
```

To be continued...


# 9.异常处理

所谓错误，就是程序调用某个函数的时候，如果失败了，就表示出错。

## Java的异常

调用方如何获知调用失败的信息？有两种方法：

方法一：约定返回错误码。

例如，处理一个文件，如果返回`0`，表示成功，返回其他整数，表示约定的错误码：

```java

int code = processFile("C:\\test.txt");
if (code == 0) {
    // ok:
} else {
    // error:
    switch (code) {
    case 1:
        // file not found:
    case 2:
        // no read permission:
    default:
        // unknown error:
    }
}

```

方法二：在语言层面上提供一个异常处理机制。

Java内置了一套异常处理机制，总是使用异常来表示错误。

异常是一种`class`，因此它本身带有类型信息。异常可以在任何地方抛出，但只需要在上层捕获，这样就和方法调用分离了：

try {
    String s = processFile(“C:\\test.txt”);
    // ok:
} catch (FileNotFoundException e) {
    // file not found:
} catch (SecurityException e) {
    // no read permission:
} catch (IOException e) {
    // io error:
} catch (Exception e) {
    // other error:
}

因为Java的异常是`class`，它的继承关系如下：

```plaintext

                       ┌───────────┐
                       │   Object   │
                       └───────────┘
                             ▲
                             │
                       ┌───────────┐
                       │ Throwable  │
                       └───────────┘
                             ▲
                  ┌──────────┴──────────┐
                  │                     │
            ┌───────────┐        ┌───────────┐
            │   Error    │        │ Exception │
            └───────────┘        └───────────┘
                  ▲                     ▲
        ┌─────────┘              ┌─────┴────────────┐
        │                        │                  │
┌─────────────────┐     ┌─────────────────┐ ┌───────────┐
│ OutOfMemoryError│ ... │ RuntimeException│ │ IOException│ ...
└─────────────────┘     └─────────────────┘ └───────────┘
                                  ▲
                 ┌────────────────┴────────────────┐
                 │                                 │
     ┌─────────────────────────┐     ┌─────────────────────────┐
     │ NullPointerException    │     │ IllegalArgumentException │ ...
     └─────────────────────────┘     └─────────────────────────┘

```

## 异常类型

`Throwable`有两个体系：`Error`和`Exception`，`Error`表示严重的错误，程序对此一般无能为力，例如：

- `OutOfMemoryError`：内存耗尽
- `NoClassDefFoundError`：无法加载某个Class
- `StackOverflowError`：栈溢出
- 
而`Exception`则是运行时的错误，它可以被捕获并处理。

某些异常是应用程序逻辑处理的一部分，应该捕获并处理。例如：

- `NumberFormatException`：数值类型的格式错误
- `FileNotFoundException`：未找到文件
- `SocketException`：读取网络失败

还有一些异常是程序逻辑编写不对造成的，应该修复程序本身。例如：

- `NullPointerException`：对某个null的对象调用方法或字段
- `IndexOutOfBoundsException`：数组索引越界

`Exception`又分为两大类：

- `RuntimeException`以及它的子类；
- 非`RuntimeException`（包括`IOException`、`ReflectiveOperationException`等等）


Java规定：

- 必须捕获的异常，包括`Exception`及其子类，但不包括`RuntimeException`及其子类，这种类型的异常称为`Checked Exception`。
- 不需要捕获的异常，包括`Error`及其子类，`RuntimeException`及其子类。



## 捕获异常

捕获异常使用`try...catch`语句，把可能发生异常的代码放到`try {...}`中，然后使用`catch`捕获对应的`Exception`及其子类

```java
// try...catch
import java.io.UnsupportedEncodingException;
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        byte[] bs = toGBK("中文");
        System.out.println(Arrays.toString(bs));
    }

    static byte[] toGBK(String s) {
        try {
            // 用指定编码转换String为byte[]:
            return s.getBytes("GBK");
        } catch (UnsupportedEncodingException e) {
            // 如果系统不支持GBK编码，会捕获到UnsupportedEncodingException:
            System.out.println(e); // 打印异常信息
            return s.getBytes(); // 尝试使用默认编码
        }
    }
}

```

如果我们不捕获`UnsupportedEncodingException`，会出现编译失败的问题：

```java

import java.io.UnsupportedEncodingException;
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        byte[] bs = toGBK("中文");
        System.out.println(Arrays.toString(bs));
    }

    static byte[] toGBK(String s) {
        return s.getBytes("GBK");
    }
}

```

这是因为`String.getBytes(String)`方法定义是：

```java
public byte[] getBytes(String charsetName) throws UnsupportedEncodingException {
    ...
}

```
在方法定义的时候，使用`throws Xxx`表示该方法可能抛出的异常类型。调用方在调用的时候，必须强制捕获这些异常，否则编译器会报错。



我们也可以直接不捕获它，而是在方法定义处用throws表示toGBK()方法可能会抛出UnsupportedEncodingException，就可以让toGBK()方法通过编译器检查：

```java

import java.io.UnsupportedEncodingException;
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        byte[] bs = toGBK("中文");
        System.out.println(Arrays.toString(bs));
    }

    static byte[] toGBK(String s) throws UnsupportedEncodingException {
        return s.getBytes("GBK");
    }
}
```

上述代码仍然会得到编译错误，修复方法是在`main()`方法中捕获异常并处理：


```java
import java.io.UnsupportedEncodingException;
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        try {
            byte[] bs = toGBK("中文");
            System.out.println(Arrays.toString(bs));
        } catch (UnsupportedEncodingException e) {
            System.out.println(e);
        }
    }

    static byte[] toGBK(String s) throws UnsupportedEncodingException {
        // 用指定编码转换String为byte[]:
        return s.getBytes("GBK");
    }
}

```

可见，只要是方法声明的`Checked Exception`，不在调用层捕获，也必须在更高的调用层捕获。所有未捕获的异常，最终也必须在`main()`方法中捕获，不会出现漏写`try`的情况。这是由编译器保证的。`main()`方法也是最后捕获`Exception`的机会。


如果是测试代码，上面的写法就略显麻烦。如果不想写任何`try`代码，可以直接把`main()`方法定义为`throws Exception`：

```java
import java.io.UnsupportedEncodingException;
import java.util.Arrays;

public class Main {
    public static void main(String[] args) throws Exception {
        byte[] bs = toGBK("中文");
        System.out.println(Arrays.toString(bs));
    }

    static byte[] toGBK(String s) throws UnsupportedEncodingException {
        // 用指定编码转换String为byte[]:
        return s.getBytes("GBK");
    }
}
```
因为`main()`方法声明了可能抛出`Exception`，也就声明了可能抛出所有的`Exception`，因此在内部就无需捕获了。代价就是一旦发生异常，程序会立刻退出。




# 10.注解

什么是注解（Annotation）？注解是放在Java源码的类、方法、字段、参数前的一种特殊“注释”：

`注释`会被编译器直接忽略，`注解`则可以被编译器打包进入class文件，因此，`注解`是一种用作标注的“元数据”。

```java
// this is a component:
@Resource("hello")
public class Hello {
    @Inject
    int n;

    @PostConstruct
    public void hello(@Param String name) {
        System.out.println(name);
    }

    @Override
    public String toString() {
        return "Hello";
    }
}

```


```

## 注解的分类

第一类是由编译器使用的注解，例如：

-`@Override`：让编译器检查该方法是否正确地实现了覆写；
-`@SuppressWarnings`：告诉编译器忽略此处代码产生的警告。
这类注解不会被编译进入.class文件，它们在编译后就被编译器扔掉了。



第二类是由工具处理`.class`文件使用的注解，比如有些工具会在加载class的时候，对class做动态修改，实现一些特殊的功能。这类注解会被编译进入`.class`文件，但加载结束后并不会存在于内存中。这类注解只被一些底层库使用，一般我们不必自己处理。

第三类是在程序运行期能够读取的注解，它们在加载后一直存在于JVM中，这也是最常用的注解。例如，一个配置了`@PostConstruct`的方法会在调用构造方法后自动被调用（这是Java代码读取该注解实现的功能，JVM并不会识别该注解）。


## 定义注解

Java语言使用`@interface`语法来定义注解（`Annotation`），它的格式如下：

```java
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```
注解的参数类似无参数方法，可以用`default`设定一个默认值（强烈推荐）。最常用的参数应当命名为`value`。

### 元注解

有一些注解可以修饰其他注解，这些注解就称为元注解（meta annotation）

最常用的元注解是`@Target`。使用`@Target`可以定义`Annotation`能够被应用于源码的哪些位置：

- 类或接口：`ElementType.TYPE`；
- 字段：`ElementType.FIELD`；
- 方法：`ElementType.METHOD`；
- 构造方法：`ElementType.CONSTRUCTOR`；
- 方法参数：`ElementType.PARAMETER`。


```java
@Target({
    ElementType.METHOD,
    ElementType.FIELD
})
public @interface Report {
    ...
}
```
`@Target`定义的`value`是`ElementType[]`数组，只有一个元素时，可以省略数组的写法。

另一个重要的元注解`@Retention`定义了`Annotation`的生命周期：

- 仅编译期：`RetentionPolicy.SOURCE`；
- 仅class文件：`RetentionPolicy.CLASS`；
- 运行期：`RetentionPolicy.RUNTIME`。

如果`@Retention`不存在，则该`Annotation`默认为`CLASS`。通常我们自定义的`Annotation`都是`RUNTIME`，所以，务必要加上`@Retention(RetentionPolicy.RUNTIME)`



`@Inherited`用于标识某个注解是可以被子类继承的。当注解使用了 `@Inherited` 时，子类自动继承父类上的该注解。


## 总结：如何定义Annotation

第一步，用`@interface`定义注解
```java
public @interface Report {
}
```
第二步，添加参数、默认值：
```java
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}

```
第三步，用元注解配置注解：
```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface Report {
    int type() default 0;
    String level() default "info";
    String value() default "";
}
```


# 11.泛型


## 一个引子

```java
public class ArrayList {
    private Object[] array;
    private int size;
    public void add(Object e) {...}
    public void remove(int index) {...}
    public Object get(int index) {...}
}
```
如果用上述`ArrayList``存储String`类型，会有这么几个缺点：
- 需要强制转型；
- 不方便，易出错。

例如，代码必须这么写：
```java
ArrayList list = new ArrayList();
list.add("Hello");
// 获取到Object，必须强制转型为String:
String first = (String) list.get(0);
```
很容易出现ClassCastException，因为容易“误转型”：
```java
list.add(new Integer(123));
// ERROR: ClassCastException:
String second = (String) list.get(1);
```

要解决上述问题，我们可以为`String`单独编写一种`ArrayList`：

```java
public class StringArrayList {
    private String[] array;
    private int size;
    public void add(String e) {...}
    public void remove(int index) {...}
    public String get(int index) {...}
}
```


这样一来，存入的必须是`String`，取出的也一定是`String`，不需要强制转型，因为编译器会强制检查放入的类型：

```java
StringArrayList list = new StringArrayList();
list.add("Hello");
String first = list.get(0);
// 编译错误: 不允许放入非String类型:
list.add(new Integer(123));
```

问题暂时解决。

然而，新的问题是，如果要存储`Integer`，还需要为`Integer`单独编写一种`ArrayList`：
```java
public class IntegerArrayList {
    private Integer[] array;
    private int size;
    public void add(Integer e) {...}
    public void remove(int index) {...}
    public Integer get(int index) {...}
}
```

实际上，还需要为其他所有`class`单独编写一种`ArrayList`：

- LongArrayList
- DoubleArrayList
- PersonArrayList
- ...

这是不可能的，JDK的class就有上千个，而且它还不知道其他人编写的class。

为了解决新的问题，我们必须把`ArrayList`变成一种模板：`ArrayList<T>`

```java
public class ArrayList<T> {
    private T[] array;
    private int size;
    public void add(T e) {...}
    public void remove(int index) {...}
    public T get(int index) {...}
}
```
`T`可以是任何class。这样一来，我们就实现了：编写一次模版，可以创建任意类型的`ArrayList`：

```java

// 创建可以存储String的ArrayList:
ArrayList<String> strList = new ArrayList<String>();
// 创建可以存储Float的ArrayList:
ArrayList<Float> floatList = new ArrayList<Float>();
// 创建可以存储Person的ArrayList:
ArrayList<Person> personList = new ArrayList<Person>();

```


# 11.集合

C++里面学过一些，暂时跳过，To be continued...


# 12.IO


# 13.单元测试

## JUnit测试

我们先通过一个示例来看如何编写测试。假定我们编写了一个计算阶乘的类，它只有一个静态方法来计算阶乘：


```java
public class Factorial {
    public static long fact(long n) {
        long r = 1;
        for (long i = 1; i <= n; i++) {
            r = r * i;
        }
        return r;
    }
}
```

以Eclipse为例，当我们已经编写了一个`Factorial.java`文件后，我们想对其进行测试，需要编写一个对应的`FactorialTest.java`文件，以`Test`为后缀是一个惯例，并分别将其放入`src`和`test`目录中。

我们来看一下`FactorialTest.java`的内容：

```java
package com.itranswarp.learnjava;

import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.Test;

public class FactorialTest {

    @Test
    void testFact() {
        assertEquals(1, Factorial.fact(1));
        assertEquals(2, Factorial.fact(2));
        assertEquals(6, Factorial.fact(3));
        assertEquals(3628800, Factorial.fact(10));
        assertEquals(2432902008176640000L, Factorial.fact(20));
    }
}
```

核心测试方法`testFact()`加上了`@Test`注解，这是JUnit要求的，它会把带有`@Test`的方法识别为测试方法。在测试方法内部，我们用`assertEquals(1, Factorial.fact(1))`表示，期望`Factorial.fact(1)`返回`1`。`assertEquals(expected, actual)`是最常用的测试方法，它在`Assertion`类中定义。`Assertion`还定义了其他断言方法，例如：

- `assertTrue()`: 期待结果为true
- `assertFalse()`: 期待结果为false
- `assertNotNull()`: 期待结果为非null
- `assertArrayEquals()`: 期待结果为数组并与期望数组每个元素的值均相等
- ...
运行单元测试非常简单。选中`FactorialTest.java`文件，点击`Run` - `Run As` - `JUnit Test`，Eclipse会自动运行这个JUnit测试，并显示结果。




# 14.多线程

# 15.Maven基础

Maven是一个Java项目管理和构建工具，它可以定义项目结构、项目依赖，并使用统一的方式进行自动化构建，是Java项目不可缺少的工具。


在了解Maven之前，我们先来看看一个Java项目需要的东西。首先，我们需要确定引入哪些依赖包。例如，如果我们需要用到commons logging，我们就必须把commons logging的jar包放入classpath。如果我们还需要log4j，就需要把log4j相关的jar包都放到classpath中。这些就是依赖包的管理。

其次，我们要确定项目的目录结构。例如，`src`目录存放Java源码，`resources`目录存放配置文件，`bin`目录存放编译生成的`.class`文件。

此外，我们还需要配置环境，例如JDK的版本，编译打包的流程，当前代码的版本号。

最后，除了使用Eclipse这样的IDE进行编译外，我们还必须能通过命令行工具进行编译，才能够让项目在一个独立的服务器上编译、测试、部署。


Maven就是是专门为Java项目打造的管理和构建工具，它的主要功能有：

- 提供了一套标准化的项目结构；
- 提供了一套标准化的构建流程（编译，测试，打包，发布……）；
- 提供了一套依赖管理机制。


## Maven 项目结构

一个使用 Maven 管理的普通 Java 项目，默认的目录结构如下所示：

- `pom.xml`：Maven 项目的配置文件，包含项目的基本信息和依赖等。
- `src/main/java`：存放主应用程序代码的目录。
- `src/main/resources`：存放资源文件（如配置文件、静态资源等）的目录。
- `src/test/java`：存放测试代码的目录。
- `src/test/resources`：存放测试相关资源文件的目录。
- `target`：编译后的输出目录，包含编译后的 class 文件、JAR 文件等。

`pom.xml` 的核心元素包括：

1.项目基本信息：
- `groupId`：指定项目所属的组织或公司。通常采用反转的域名格式。
- `artifactId`：指定项目的唯一名称。
- `version`：指定项目的版本号。
- `packaging`：指定构建输出的格式（例如 `jar`、`war`、`pom` 等）。

  
2.依赖管理：

- `<dependencies>`：列出项目的外部依赖，Maven 会自动下载这些依赖库。

3.插件和构建配置：

- `<build>`：指定构建过程中的插件和其他构建相关的配置。
- `<plugins>`：定义构建时需要使用的插件（如编译插件、测试插件等）。

4.其他配置：

- `<properties>`：定义一些项目的属性（如编码格式、Java 版本等）。


示例

```java
<project ...>
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.itranswarp.learnjava</groupId>
	<artifactId>hello</artifactId>
	<version>1.0</version>
	<packaging>jar</packaging>
	<properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<maven.compiler.release>17</maven.compiler.release>
	</properties>
	<dependencies>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-simple</artifactId>
            <version>2.0.16</version>
        </dependency>
	</dependencies>
</project>
```


### 依赖

在 Maven 中，依赖（Dependency）指的是项目在构建、编译或运行时所需要的外部库或模块。简单来说，依赖就是你项目中引用的第三方代码库，或者是项目之间相互引用的模块。Maven 通过 `pom.xml` 文件来声明这些依赖，Maven 会自动下载并管理这些依赖，确保它们正确地集成到项目中。

```java
<dependencies>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>1.7.32</version>
    </dependency>
</dependencies>
```

- `groupId`：依赖所在的组织或公司名称。通常是反转的域名，例如 org.slf4j。
- `artifactId`：依赖的具体模块或库的名称。在这个例子中是 `slf4j-api`，表示 `SLF4J` 的 API 库。
- `version`：指定依赖的版本号。这样 Maven 会知道需要下载哪个版本的库。





## 模块管理
在软件开发中，把一个大项目分拆为多个模块是降低软件复杂度的有效方法：

```plaintext
                        ┌──────────────┐
                        │ Single       │
                        │ Project      │
                        └──────────────┘
                               │
                   ┌───────────┴───────────┐
                   │           │           │
            ┌──────────┐ ┌──────────┐ ┌──────────┐
            │ Module A │ │ Module B │ │ Module C │
            └──────────┘ └──────────┘ └──────────┘
```


对于Maven工程来说，原来是一个大项目：
```plaintext
single-project
├── pom.xml
└── src
```

现在可以分拆成3个模块：

```plaintext
multiple-projects
├── module-a
│   ├── pom.xml
│   └── src
├── module-b
│   ├── pom.xml
│   └── src
└── module-c
    ├── pom.xml

```

Maven可以有效地管理多个模块，我们只需要把每个模块当作一个独立的Maven项目，它们有各自独立的`pom.xml`

## 使用mvnw

我们使用Maven时，基本上只会用到`mvn`这一个命令
`mvnw`是Maven Wrapper的缩写。简单地说，Maven Wrapper就是给一个项目提供一个独立的，指定版本的Maven给它使用。

### 安装Maven Wrapper

安装Maven Wrapper最简单的方式是在项目的根目录（即`pom.xml`所在的目录）下运行安装命令：
```
mvn wrapper:wrapper
```

它会自动使用最新版本的Maven。如果要指定使用的Maven版本，使用下面的安装命令指定版本，例如`3.9.0`：
```
$ mvn wrapper:wrapper -Dmaven=3.9.0
```
安装后，查看项目结构：

```plaintext
my-project
├── .mvn
│   └── wrapper
│       └── maven-wrapper.properties
├── mvnw
├── mvnw.cmd
├── pom.xml
└── src
    ├── main
    │   ├── java
    │   └── resources
    └── test
        ├── java
        └── resources
```

# 网络编程

## IP地址

在互联网中，一个IP地址用于唯一标识一个网络接口（Network Interface）。一台联入互联网的计算机肯定有一个IP地址，但也可能有多个IP地址。

IP地址分为IPv4和IPv6两种。IPv4采用32位地址，类似`101.202.99.12`，而IPv6采用128位地址，类似`2001:0DA8:100A:0000:0000:1020:F2F3:1428`。IPv4地址总共有232个（大约42亿），而IPv6地址则总共有2128个（大约340万亿亿亿亿），IPv4的地址目前已耗尽，而IPv6的地址是根本用不完的。


## TCP编程

### Socket

Socket是一个抽象概念，一个应用程序通过一个Socket来建立一个远程连接，而Socket内部通过TCP/IP协议把数据传输到网络：

一个Socket就是由IP地址和端口号（范围是0～65535）组成，可以把Socket简单理解为IP地址加端口号。端口号总是由操作系统分配，它是一个0～65535之间的数字，其中，小于1024的端口属于特权端口，需要管理员权限，大于1024的端口可以由任意用户的应用程序打开。

- Browser: 101.202.99.2:1201
- QQ: 101.202.99.2:1304
- Email: 101.202.99.2:15000

因此，当Socket连接成功地在服务器端和客户端之间建立后：

- 对服务器端来说，它的Socket是指定的IP地址和指定的端口号；
- 对客户端来说，它的Socket是它所在计算机的IP地址和一个由操作系统分配的随机端口号。


```plaintext
┌───────────┐                                ┌───────────┐
│Application│                                │Application│
├───────────┤                                ├───────────┤
│  Socket   │                                │  Socket   │
├───────────┤                                ├───────────┤
│    TCP    │                                │    TCP    │
├───────────┤     ┌──────┐      ┌──────┐     ├───────────┤
│    IP     │◀───▶│Router│◀────▶│Router│◀───▶│    IP     │
└───────────┘     └──────┘      └──────┘     └───────────┘

```


### 服务器端

要使用Socket编程，我们首先要编写服务器端程序。Java标准库提供了`ServerSocket`来实现对指定IP和指定端口的监听。`ServerSocket`的典型实现代码如下：


```java
public class Server {
    public static void main(String[] args) throws IOException {
        ServerSocket ss = new ServerSocket(6666); // 监听指定端口
        System.out.println("server is running...");
        for (;;) {
            Socket sock = ss.accept();
            System.out.println("connected from " + sock.getRemoteSocketAddress());
            Thread t = new Handler(sock);
            t.start();
        }
    }
}
```

```java
class Handler extends Thread {
    Socket sock;

    public Handler(Socket sock) {
        this.sock = sock;
    }

    @Override
    public void run() {
        try (InputStream input = this.sock.getInputStream()) {
            try (OutputStream output = this.sock.getOutputStream()) {
                handle(input, output);
            }
        } catch (Exception e) {
            try {
                this.sock.close();
            } catch (IOException ioe) {
            }
            System.out.println("client disconnected.");
        }
    }

    private void handle(InputStream input, OutputStream output) throws IOException {
        var writer = new BufferedWriter(new OutputStreamWriter(output, StandardCharsets.UTF_8));
        var reader = new BufferedReader(new InputStreamReader(input, StandardCharsets.UTF_8));
        writer.write("hello\n");
        writer.flush();
        for (;;) {
            String s = reader.readLine();
            if (s.equals("bye")) {
                writer.write("bye\n");
                writer.flush();
                break;
            }
            writer.write("ok: " + s + "\n");
            writer.flush();
        }
    }
}
```
### 客户端

```java
public class Client {
    public static void main(String[] args) throws IOException {
        Socket sock = new Socket("localhost", 6666); // 连接指定服务器和端口
        try (InputStream input = sock.getInputStream()) {
            try (OutputStream output = sock.getOutputStream()) {
                handle(input, output);
            }
        }
        sock.close();
        System.out.println("disconnected.");
    }

    private static void handle(InputStream input, OutputStream output) throws IOException {
        var writer = new BufferedWriter(new OutputStreamWriter(output, StandardCharsets.UTF_8));
        var reader = new BufferedReader(new InputStreamReader(input, StandardCharsets.UTF_8));
        Scanner scanner = new Scanner(System.in);
        System.out.println("[server] " + reader.readLine());
        for (;;) {
            System.out.print(">>> "); // 打印提示
            String s = scanner.nextLine(); // 读取一行输入
            writer.write(s);
            writer.newLine();
            writer.flush();
            String resp = reader.readLine();
            System.out.println("<<< " + resp);
            if (resp.equals("bye")) {
                break;
            }
        }
    }
}
```

# 17.XML和JSON

## JSON

在Web上使用XML现在越来越少，取而代之的是JSON这种数据结构。

JSON是JavaScript Object Notation的缩写，它去除了所有JavaScript执行代码，只保留JavaScript的对象格式。一个典型的JSON如下：

```java
{
    "id": 1,
    "name": "Java核心技术",
    "author": {
        "firstName": "Abc",
        "lastName": "Xyz"
    },
    "isbn": "1234567",
    "tags": ["Java", "Network"]
}

```

# 18.JDBC编程

## 引子
程序运行的时候，数据都是在内存中的。当程序终止的时候，通常都需要将数据保存到磁盘上，无论是保存到本地磁盘，还是通过网络保存到服务器上，最终都会将数据写入磁盘文件。

而如何定义数据的存储格式就是一个大问题。

你可以用一个文本文件保存，一行保存一个学生，用,隔开：

``` plaintext
Michael,99
Bob,85
Bart,59
Lisa,87
```

你还可以用JSON格式保存，也是文本文件：
``` plaintext
[
    {"name":"Michael","score":99},
    {"name":"Bob","score":85},
    {"name":"Bart","score":59},
    {"name":"Lisa","score":87}
]
```

但是问题来了：

不能做快速查询，只有把数据全部读到内存中才能自己遍历，但有时候数据的大小远远超过了内存（比如蓝光电影，40GB的数据），根本无法全部读入内存。

为了便于程序保存和读取数据，而且，能直接通过条件快速查询到指定的数据，就出现了数据库（Database）这种专门用于集中存储和查询的软件。





## 数据库类别
既然我们要使用关系数据库，就必须选择一个关系数据库。目前广泛使用的关系数据库也就这么几种：

付费的商用数据库：

- Oracle，典型的高富帅；
- SQL Server，微软自家产品，Windows定制专款；
- DB2，IBM的产品，听起来挺高端；
- Sybase，曾经跟微软是好基友，后来关系破裂，现在家境惨淡。
  
这些数据库都是不开源而且付费的，最大的好处是花了钱出了问题可以找厂家解决，不过在Web的世界里，常常需要部署成千上万的数据库服务器，当然不能把大把大把的银子扔给厂家，所以，无论是Google、Facebook，还是国内的BAT，无一例外都选择了免费的开源数据库：

- MySQL，大家都在用，一般错不了；
- PostgreSQL，学术气息有点重，其实挺不错，但知名度没有MySQL高；
- sqlite，嵌入式数据库，适合桌面和移动应用。

作为一个Java工程师，选择哪个免费数据库呢？当然是MySQL。因为MySQL普及率最高，出了错，可以很容易找到解决方法。而且，围绕MySQL有一大堆监控和运维的工具，安装和使用很方便。


## JDBC

什么是JDBC？JDBC是Java DataBase Connectivity的缩写，它是Java程序访问数据库的标准接口。

使用Java程序访问数据库时，Java代码并不是直接通过TCP连接去访问数据库，而是通过JDBC接口来访问，而JDBC接口则通过JDBC驱动来实现真正对数据库的访问。

使用JDBC的好处是：

- 各数据库厂商使用相同的接口，Java代码不需要针对不同数据库分别开发；
- Java程序编译期仅依赖java.sql包，不依赖具体数据库的jar包；
- 可随时替换底层数据库，访问数据库的Java代码基本不变。



# 19.函数式编程

函数式编程（请注意多了一个“式”字）——Functional Programming，虽然也可以归结到面向过程的程序设计，但其思想更接近数学计算。

函数式编程的一个特点就是，允许把函数本身作为参数传入另一个函数，还允许返回一个函数！

Java平台从Java 8开始，支持函数式编程。


## Lambda基础

Lambda 表达式允许将代码块传递给方法作为参数，使得 Java 编程更加简洁和灵活。其基本语法如下：

```java
(parameters) -> expression
```
例如：

```java
// 传统方式
Runnable r1 = new Runnable() {
    @Override
    public void run() {
        System.out.println("Hello, World!");
    }
};

// 使用 Lambda 表达式
Runnable r2 = () -> System.out.println("Hello, World!");

```

解析：
- `Runnable` 是一个函数式接口，它只有一个方法 `run()`，这个方法没有参数，也没有返回值。
- `Runnable r1 = new Runnable() {...};` 这部分代码使用了 匿名内部类（Anonymous Inner Class）的方式来实现 `Runnable` 接口。
- 这种方式虽然能够实现 `Runnable` 接口，但它较为冗长，需要显式地创建一个匿名内部类，并且定义接口方法的实现。这对于一个简单的任务来说显得有些繁琐。

使用 Lambda 表达式的方式

```java
Runnable r2 = () -> System.out.println("Hello, World!");

```
解析：
- Lambda 表达式：Java 8 引入了 Lambda 表达式，使得我们能够以更简洁的方式表达匿名类的实现。
- `Runnable` 是一个函数式接口，它只有一个抽象方法 run()，因此可以使用 Lambda 表达式来替代传统的匿名内部类。
- `()`：表示 `run()` 方法的参数列表。由于 `run()` 方法没有参数，这里为空。
- `->`：是 Lambda 表达式的分隔符，表示将左边的输入参数（这里为空）映射到右边的函数体。
- `System.out.println("Hello, World!");`：这是 Lambda 表达式的主体，表示执行的代码。当 `run()`方法被调用时，这行代码会被执行，打印出 "Hello, World!"。

# 20.设计模式

设计模式，即Design Patterns，是指在软件设计中，被反复使用的一种代码设计经验。使用设计模式的目的是为了可重用代码，提高代码的可扩展性和可维护性。

## 开闭原则
在增加新功能的时候，能不改代码就尽量不要改，如果只增加代码就完成了新功能，那是最好的。

## 里氏替换原则
这是一种面向对象的设计原则，即如果我们调用一个父类的方法可以成功，那么替换成子类调用也应该完全可以运行。

学习设计模式，关键是学习设计思想，不能简单地生搬硬套，也不能为了使用设计模式而过度设计，要合理平衡设计的复杂度和灵活性，并意识到设计模式也并不是万能的。


# 21.web开发

正式进入到JavaEE的领域。

什么是JavaEE？JavaEE是Java Platform Enterprise Edition的缩写，即Java企业平台。我们前面介绍的所有基于标准JDK的开发都是JavaSE，即Java Platform Standard Edition。此外，还有一个小众不太常用的JavaME：Java Platform Micro Edition，是Java移动开发平台（非Android），它们三者关系如下：

```plaintext
┌────────────────┐
│     JavaEE     │
│┌──────────────┐│
││    JavaSE    ││
││┌────────────┐││
│││   JavaME   │││
││└────────────┘││
│└──────────────┘│
└────────────────┘

```

目前流行的基于Spring的轻量级JavaEE开发架构，使用最广泛的是Servlet和JMS，以及一系列开源组件。本章我们将详细介绍基于Servlet的Web开发。



## web基础

我们访问网站，使用App时，都是基于Web这种Browser/Server模式，简称BS架构，它的特点是，客户端只需要浏览器，应用程序的逻辑和数据都存储在服务器端。浏览器只需要请求服务器，获取Web页面，并把Web页面展示给用户即可。

Web页面具有极强的交互性。由于Web页面是用HTML编写的，而HTML具备超强的表现力，并且，服务器端升级后，客户端无需任何部署就可以使用到新的版本，因此，BS架构升级非常容易。

### HTTP协议

在Web应用中，浏览器请求一个URL，服务器就把生成的HTML网页发送给浏览器，而浏览器和服务器之间的传输协议是HTTP，所以：

- HTML是一种用来定义网页的文本，会HTML，就可以编写网页；
- HTTP是在网络上传输HTML的协议，用于浏览器和服务器的通信。
- 
HTTP协议是一个基于TCP协议之上的请求-响应协议，它非常简单，我们先使用Chrome浏览器查看新浪首页，然后选择View - Developer - Inspect Elements就可以看到HTML：

对于Browser来说，请求页面的流程如下：

- 与服务器建立TCP连接；
- 发送HTTP请求；
- 收取HTTP响应，然后把网页在浏览器中显示出来。



# 22.Spring开发

# 23.Spring Boot开发





# 实时补充


## `enum`

`enum`（枚举）是 Java 中的一种特殊的类，它表示一组常量（例如，星期天到星期六、季节等）。`enum` 可以在 Java 中用于表示固定的一组常量，并且具有更强的类型安全性。

假设我们有一个 `Season` 枚举，表示一年中的四个季节

```java
public enum Season {
    SPRING, SUMMER, AUTUMN, WINTER;
}
```
示例：每个季节有不同的温度范围

```java

public enum Season {
    SPRING(15, 25), // 春季温度范围：15到25
    SUMMER(25, 35), // 夏季温度范围：25到35
    AUTUMN(10, 20), // 秋季温度范围：10到20
    WINTER(-5, 10); // 冬季温度范围：-5到10

    private final int minTemp; // 最低温度
    private final int maxTemp; // 最高温度

    // 构造方法
    Season(int minTemp, int maxTemp) {
        this.minTemp = minTemp;
        this.maxTemp = maxTemp;
    }

    // 获取最低温度
    public int getMinTemp() {
        return minTemp;
    }

    // 获取最高温度
    public int getMaxTemp() {
        return maxTemp;
    }

    // 输出季节的温度范围
    @Override
    public String toString() {
        return this.name() + ": " + minTemp + "°C to " + maxTemp + "°C";
    }
}
```

那么要如何用我们定义的这个`enum`呢？

```java
public class EnumExample {
    public static void main(String[] args) {
        // 输出所有季节的温度范围
        for (Season season : Season.values()) {
            System.out.println(season);
        }
    }
}
```

现在问个问题，为什么我们可以直接打印`season`对象？

