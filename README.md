# StudyNotes
This is a programming learning record

# 1. 在Java中，访问控制修饰符

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


