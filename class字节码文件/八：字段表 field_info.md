# 八：字段表 field_info



```json
紧跟着的是'interfaces' 后面的是字段表，结构如下：
	｛
		field_info_count:
		field_info:{}
	｝
因为一个类或者接口中字段可能存在多个

字段表用于描述类或者接口中的变量，字段：类中的实例变量，类变量（静态变量）但是不包括方法中的局部变量
字段表描述字段的那些信息呢？
	字段的作用域（public,private,protected）
	字段的从属（类变量，还是实例变量）
	字段的可变性（final）
	字段的数据类型（基本类型，数组，引用类型）
	字段并发可见性（volatile）
	字段可否被序列化
上述的这些除了字段的数据类型，其他的都是可以用标志位来表示的，
而字段的数据类型，字段的简单名称，都是无法固定的，只能引用常量池中的CONSTANT_Class_info 或者其他基本类型数据信息

字段表的结构如下,表中的数据都已一一排列的，access_flags 之后就是name_index，依次类推
我们知道
```

| 名称             | 类型           | 数量                |
| ---------------- | -------------- | ------------------- |
| access_flags     | u2             | 1                   |
| name_index       | u2             | 1                   |
| descriptor_index | u2             | 1                   |
| attributes_count | u2             | 1                   |
| attributes       | attribute_info | ${attributes_count} |



> #### access_flag 描述字段的信息

| 标志名称      | 标志值 | 含义               |
| ------------- | ------ | ------------------ |
| ACC_PUBLIC    | 0x0001 | 是否是public       |
| ACC_PRIVATE   | 0x0002 | 是否是private      |
| ACC_PROTECTED | 0x0004 | 是否是protected    |
| ACC_STATIC    | 0x0008 | 是否是static       |
| ACC_FINAL     | 0x0010 | 是否是final        |
| ACC_VOLATILE  | 0x0040 | 是否是volatile     |
| ACC_TRANSIENT | 0x0080 | 是否是transient    |
| ACC_SYNTHETIC | 0x1000 | 是否由编译器添加的 |
| ACC_ENUM      | 0x4000 | 是否是enum         |

```json
public private protected 是互斥的
final,volatile 是互斥的
接口中的字段必须是 public static final 修饰的
```



> ### name_index

```json
name_inde 是对常量池中的字符常量的引用 name_index 引用的就是字段的简单名称
```



> ### descriptor_index

```json
descriptor_index 就是对常量池中字符常量的引用，该引用就是字段的描述符如:
	数组变量 int a[],描述符就是[I
	字符变量，C
    引用变量,Ljava.util.List；
	
```



> ### attributes_count

```json
紧接着descriptor_index 的就是字段的属性表了，属性表的第一项attributes_count 指明了该字段有哪些属性，attributes_count可以为0，也就是说字段表中的属性表是可省略，不存在，也可以节选的，'attributes_count'肯定在字节码中占据空间紧跟着'desscriptor_index'，如果为0 则属性表不存在，但是也要占据字节码文件的空间去告诉JVM 该字段没有其他属性
attribute_info 字段属性表的第二项，字段属性表有哪些内容呢？
```

| 属性名称        | 应用位置           | 含义                                                         |
| --------------- | ------------------ | ------------------------------------------------------------ |
| Code            | 方法表             | 方法被翻译为字节码指令                                       |
| ConstantValue   | 字段表             | final 关键字修饰的变量的值                                   |
| Deprecated      | 类，方法表，字段表 | 被声明为弃用的字段，方法                                     |
| Exceptions      | 方法表             | 方法抛出的异常                                               |
| EnClosingMethod | 类文件             | 只有局部内部类，匿名内部类拥有这个属性，用于标注该内部类所在外围方法 |

```json
在字段表中 attributes_count的最大值是2，也就是ConstantValue，和Deprecated
其中ConstantValue=3,
是final 修饰的变量的值，变量有可能是基本类型变量，或者引用类型变量，
如：
	final static int a=3; 那么字段a 的ConstantValue=3,
	在类加载的时候初始化的时候就会被赋予a=ConstantValue
	那有什么用呢？
	static int a=3,在类被加载的时候初始化 显示赋予a int型初始值0 ，然后在
	<clinit>方法中被赋值a=3,
	也就是说有ConstantValue 属性的值，和静态变量的初始化不同

```

