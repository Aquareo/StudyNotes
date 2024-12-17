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
