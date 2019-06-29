# 六：access_flag 访问标志



```json
视线回到java字节码中，class字节码中的主项，都是一一排列的，且每中类型的结构只出现一次，和常量池中每种类型可能出现多次不一样的，
class 字节码中，紧跟着常量池后面的是accss_flag 是描述类的访问标志的
所谓的类的访问标志，就是该类是普通类，还是接口 | 访问权限是 public | 是否是abstract 修饰的抽象类，如果不是抽象类，是否是final 修饰的
accss_flag（u2） 的所有标记位，和标记如下表
access_flag 就一个数据，不是向常量池中的那种表结构
```

| 标志名称       | 标志值 | 含义             |
| -------------- | ------ | ---------------- |
| ACC_PUBLIC     | 0x0001 | 有public 修饰    |
| ACC_FINAL      | 0x0010 | 有final 修饰     |
| ACC_SUPER      | 0x0020 | '历史遗留'       |
| ACC_INTERFACE  | 0x0200 | 是否是接口       |
| ACC_ABSTRACT   | 0x0400 | 是不是抽象类     |
| ACC_SYNTHETIC  | 0x1000 | 是不是代码生成的 |
| ACC_ANNOTATION | 0X2000 | 是不是注解类     |
| ACC_ENUM       | 0x4000 | 是不是枚举类     |



```json
不知道有没有注意到一个类或者接口可能拥有多个标志，比如'public abstract class' 一个public 抽象类，同时拥有 ACC_PUBLIC,ACC_ABSTRACT 标志，那么，access_flag 怎么标记呢？
	表中的每个标记的标志值都是只占据低字节的低位或高位，高字节的低位或高位，其他位都是置0 的，
	还有一个特性是，一个类不可能同时拥有ACC_ABSTRACT与ACC_INTERFACE两者是互斥的，ACC_ABSTRACT与ACC_FINAL 也是互斥的，
表中的标志的组合的可能是有限的，而且还有两种特性，某标志的标志值的其他位置0，某些标志互斥，根据这些特性可以推断出access_flag 到底拥有哪些标志
```



