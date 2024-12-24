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
