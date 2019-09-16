# java中的反射

Java 反射机制主要提供了以下功能，这些功能都位于 java.lang.reflect 包中。通过反射我们主要可以做一下几件事

- 在运行时判断任意一个对象所属的类。
- 在运行时构造任意一个类的对象。
- 在运行时判断任意一个类所具有的成员变量和方法。
- 在运行时调用任意一个对象的方法。
- 生成动态代理。

众所周知，所有 Java 类均继承了 Object 类，在 Object 类中定义了一个 getClass() 方法，该方法返回同一个类型为 Class 的对象。例如，下面的示例代码：

```
Class clazz=label1.getClass();    //label1为类的对象
```

利用 Class 类的对象 clazz 可以访问 label1对象的描述信息、clazz类的信息以及基类 Object 的信息。表 1 列出了通过反射可以访问的信息。

| 类型           | 访问方法                  | 返回值类型         | 说明                                              |
| -------------- | ------------------------- | ------------------ | ------------------------------------------------- |
| 包路径         | getPackage()              | Package 对象       | 获取该类的存放路径                                |
| 类名称         | getName()                 | String 对象        | 获取该类的名称                                    |
| 继承类         | getSuperclass()           | Class 对象         | 获取该类继承的类                                  |
| 实现接口       | getlnterfaces()           | Class 型数组       | 获取该类实现的所有接口                            |
| 构造方法       | getConstructors()         | Constructor 型数组 | 获取所有权限为 public 的构造方法                  |
|                | getDeclaredContxuectors() | Constructor 对象   | 获取当前对象的所有构造方法                        |
| 方法           | getMethods()              | Methods 型数组     | 获取所有权限为 public 的方法                      |
|                | getDeclaredMethods()      | Methods 对象       | 获取当前对象的所有方法                            |
| 成员变量       | getFields()               | Field 型数组       | 获取所有权限为 public 的成员变量                  |
|                | getDeclareFileds()        | Field 对象         | 获取当前对象的所有成员变量                        |
| 内部类         | getClasses()              | Class 型数组       | 获取所有权限为 public 的内部类                    |
|                | getDeclaredClasses()      | Class 型数组       | 获取所有内部类                                    |
| 内部类的声明类 | getDeclaringClass()       | Class 对象         | 如果该类为内部类，则返回它的成员类，否则返回 null |

如表 1 所示，在调用 getFields() 和 getMethods() 方法时将会依次获取权限为 public 的字段和变量，然后将包含从超类中继承到的成员实量和方法。而通过 getDeclareFields() 和 getDeclareMethod()只是获取在本类中定义的成员变量和方法。

## 反射调用构造方法

为了能够动态获取对象构造方法的信息，首先需要通过下列方法之一创建一个 Constructor 类型的对象或者数组。

- getConstructors()
- getConstructor(Class<?>…parameterTypes)
- getDeclaredConstructors()
- getDeclaredConstructor(Class<?>...parameterTypes)

创建的每个 Constructor 对象表示一个构造方法，然后利用 Constructor 对象的方法操作构造方法。Constructor 类的常用方法如表 1 所示。

| 方法名称                       | 说明                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| isVarArgs()                    | 查看该构造方法是否允许带可变数量的参数，如果允许，返回 true，否则返回 false |
| getParameterTypes()            | 按照声明顺序以 Class 数组的形式获取该构造方法各个参数的类型  |
| getExceptionTypes()            | 以 Class 数组的形式获取该构造方法可能抛出的异常类型          |
| newInstance(Object … initargs) | 通过该构造方法利用指定参数创建一个该类型的对象，如果未设置参数则表示 采用默认无参的构造方法 |
| setAccessiable(boolean flag)   | 如果该构造方法的权限为 private，默认为不允许通过反射利用 netlnstance() 方法创建对象。如果先执行该方法，并将入口参数设置为 true，则允许创建对 象 |
| getModifiers()                 | 获得可以解析出该构造方法所采用修饰符的整数                   |

通过 java.lang.reflect.Modifier 类可以解析出 getMocMers() 方法的返回值所表示的修饰符信息。在该类中提供了一系列用来解析的静态方法，既可以查看是否被指定的修饰符修饰，还可以字符串的形式获得所有修饰符。表 2 列出了 Modifier 类的常用静态方法。

| 静态方法名称         | 说明                                                     |
| -------------------- | -------------------------------------------------------- |
| isStatic(int mod)    | 如果使用 static 修饰符修饰则返回 true，否则返回 false    |
| isPublic(int mod)    | 如果使用 public 修饰符修饰则返回 true，否则返回 false    |
| isProtected(int mod) | 如果使用 protected 修饰符修饰则返回 true，否则返回 false |
| isPrivate(int mod)   | 如果使用 private 修饰符修饰则返回 true，否则返回 false   |
| isFinal(int mod)     | 如果使用 final 修饰符修饰则返回 true，否则返回 false     |
| toString(int mod)    | 以字符串形式返回所有修饰符                               |



