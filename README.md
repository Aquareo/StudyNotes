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
