# ENUM 枚举



> ### 什么是枚举



```java
//下面定义一个枚举类，

public enum Color {
	a,b,c
}

//下面看字节码文件:
 #3 = Class              #4             // java/lang/Enum
super_class # 3

得出第一个结论枚举类Color 的父类是ENUM
```

![1561717681728](C:\Users\Administrator.MS-201605312249\AppData\Roaming\Typora\typora-user-images\1561717681728.png)

```java
明明Color类中只定义了三个字段为什么会有第四个字段的出现呢？
```

![1561717967873](C:\Users\Administrator.MS-201605312249\AppData\Roaming\Typora\typora-user-images\1561717967873.png)

```java
我们定义的 a,b,c 的类型都是Color 类型的变量，访问修饰符都是public static final enum 
第四个字段ENUM$VALUES 是private static final enum 也就是不可访问的
我们知道final static 修饰的变量必须在<clinit> 中实例化，且不能为null，要不然没有意义了
下面是<clinit>的字节码：
```

![1561718743511](C:\Users\Administrator.MS-201605312249\AppData\Roaming\Typora\typora-user-images\1561718743511.png)

```java
也就是说我声明的变量a,b,c 在编译期处理后变成了
public static final enum Color a=new Color();
public static final enum Color b=new Color();
public static final enum Color c=new Color();
第四个变量"ENUM$VALUES" 没看见实例化，仔细找了一下发现，如下指令:
```

![1561719025514](C:\Users\Administrator.MS-201605312249\AppData\Roaming\Typora\typora-user-images\1561719025514.png)

```java
//即时不懂java字节码也很容易猜出来
anewarry 创建一个Color 类型的数组
aastore 	FIELD a
aastore 	FIELD b
aastore 	FIELD c
putStatic	FILED ENUM$VALUES
大白话的意思就是创建一个Color 数组将a,b,c 放进去把数组的引用付给ENUM$VALUES 
也就是说ENUM$VALUES 是一个私用的Color 数组
```







![1561717822512](C:\Users\Administrator.MS-201605312249\AppData\Roaming\Typora\typora-user-images\1561717822512.png)

```java
//除了<clinit> <init> 方法 是每个类都有的，而且编译器还添加了两个,先看values() 方法
```

![1561719329939](C:\Users\Administrator.MS-201605312249\AppData\Roaming\Typora\typora-user-images\1561719329939.png)

```java
很明显就是将ENUM$VALUES的引用返回出去，
```

![1561719450909](C:\Users\Administrator.MS-201605312249\AppData\Roaming\Typora\typora-user-images\1561719450909.png)

```java
虽然看不懂字节码指令的具体意思，但是接受一个String 字符串然后checkcast Color,转为一个Color 类型的，肯定是根据字符串查找指定字段（a,b,c），然后返回的
```





```java
枚举类还有属性表用于描述类属性的，见下图
```

![1561734610792](C:\Users\Administrator.MS-201605312249\AppData\Roaming\Typora\typora-user-images\1561734610792.png)

```java
//居然有Singnature属性，这个属性是保存泛型信息的啊，这个源代码中没有关于泛型的啊，除非编译期做了啥，使用javap -verbose，看一下字节码文件
```

![1561734738519](C:\Users\Administrator.MS-201605312249\AppData\Roaming\Typora\typora-user-images\1561734738519.png)

```java
//可以看到 这个枚举类的声明是 public final class Color extends Enum<Color>，上面只是说过Enum 是所有枚举的父类，但是没介绍Enum 的源代码，下面看一下Enum 的声明
public abstract class Enum<E extends Enum<E>> implements Comparable<E>, Serializable{}
Enum 是个泛型类，那么继承Enum 就要传入参数化类型变量
```

