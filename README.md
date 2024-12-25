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

## 异常类型

## 捕获异常



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


我们再来看最关键的一个项目描述文件`pom.xml`，它的内容长得像下面：

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

```java
<dependencies>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>1.7.32</version>
    </dependency>
</dependencies>
```


# 16.XML和JSON

# 17.JDBC编程

# 18.函数式编程

# 19.设计模式

# 20.web开发

# 21.Spring开发

# 22.Spring Boot开发
