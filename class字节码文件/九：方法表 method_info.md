# 九：方法表 method_info



```json
方法表紧跟着field_info 排列，结构如下：
	｛
		method_info_count:
		method_info:{}
	｝
因为一个接口或者类中，方法可能存在多个
字段表用于描述类或者接口中的方法，class字节码文件中对方法表的描述和对字段表的描述几乎一致，

方法表的结构和字段表的结构差不多，都有access_flags（不过标志名称不同的），名称索引（name_index），描述符索引（descriptor_index）,属性表(attributes)

方法表中不会出现父类中的方法，只描述存在本类或者接口中的方法

表结构如下：
```

| 名称             | 类型           | 数量                |
| ---------------- | -------------- | ------------------- |
| access_flags     | u2             | 1                   |
| name_index       | u2             | 1                   |
| descriptor_index | u2             | 1                   |
| attributes_count | u2             | 1                   |
| attributes       | attribute_info | ${attributes_count} |





> ### access_flags 如下：

| 标志名称         | 标志值 | 含义                             |
| ---------------- | ------ | -------------------------------- |
| ACC_PUBLIC       | 0x0001 | 是否是public                     |
| ACC_PRIVATE      | 0x0002 | 是否是private                    |
| ACC_PROTECTED    | 0x0004 | 是否是protected                  |
| ACC_STATIC       | 0x0008 | 是否是static                     |
| ACC_FINAL        | 0x0010 | 是否是final                      |
| ACC_SYNCHRONIZED | 0x0020 | 是否是synchronized               |
| ACCBRIDGE        | 0x0040 | 是不是编译器针对泛型生成的桥方法 |
| ACC_VARARGS      | 0x0080 | 该方法是否有接收不定长参数       |
| ACC_NATIVE       | 0x0100 | 是不是本地方法                   |
| ACC_ABSTRACT     | 0x0400 | 是不是abstract                   |
| ACC_STRICTFP     | 0x0800 | 是不是strictfp                   |
| ACC_SYNTHETIC    | 0x1000 | 是不是编译器添加的方法           |





> ## 方法表的属性表

```json
属性表的结构如下： 最上面的attribute_count 指的就是下表在方法表中有几个
```

| 名称       | 含义                     |
| ---------- | ------------------------ |
| name_index | 索引常量池中的属性的名称 |
| length     | 该属性占据的长度         |
| bytes      | ${lenght}                |



> ### 方法表中的属性表可能有哪些属性

| 属性名称   | 含义                     |
| ---------- | ------------------------ |
| Code       | 方法被翻译为字节码指令   |
| Deprecated | 被声明为弃用的字段，方法 |
| Exceptions | 方法抛出的异常           |

```json
和字段表中的属性表有一样的特性，方法表中的属性表得属性并不是全部都存在的，而是节选的，
方法表可能也没有属性表，因为，如果是接口中的方法，或者抽象类中的方法没有方法体，也就没有被翻译后的字节码指令了，也就不用code属性去存储了

但是'attribute_count' 肯定会存在，意义在于告诉JVM 该方法是否有属性表

那么JVM属性表中有哪些属性呢？
	由于属性表中的属性是节选的所以不可以使用固定的排序方式确定属性是那个？所以需要name_index 去常量池与索引常量值，确定属性名
```

