# StudyNotes
This is a programming learning record

# 在Java中，访问控制修饰符

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
## 多态概念
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


如果父类Person的run()方法没有实际意义，能否去掉方法的执行语句？不行

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

# Static的作用

## 实例方法与静态方法的区别

在 Java 中，方法可以分为两类：实例方法和静态方法。这两种方法的使用方式和它们与类的关系有所不同。

### 1.1 实例方法 (Non-static Method)
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


### 1.2 静态方法 (Static Method)

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



# 多态的概念
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
### 1.1 向上转型（Upcasting）

```java
makelove a = new footjob();
```
其实是一个隐式的向上转型。```footjob``` 是 ```makelove``` 的子类，子类对象本来就包含父类的所有方法和字段。

向上转型是安全的，因为子类对象一定具备父类的结构。即使你只用父类引用，也能访问父类定义的属性和方法。

向上转型的好处是你可以通过父类的引用来操作不同子类的对象，这也是 多态 的体现。

### 1.2 访问限制

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


分析，一个实际类型为`Student`，引用类型为`Person`的变量，调用其`run()`方法，调用的是`Person`还是`Student`的run()方法？

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

## 抽象类 vs 接口：区别

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

