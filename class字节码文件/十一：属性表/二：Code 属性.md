# 二：code 属性



> code 属性的简单介绍

```json
code 属性附着在方法表中，code 属性的主要作用是存放方法编译后的字节码指令，但也并不是所有的方法表都有这个属性，如接口或者抽象类的方法没有方法体，也谈不上有编译后的字节码指令，
code 属性并不是一维的，code 属性还有描述code 属性的属性，如方法中try catch 的异常，局部变量表，调用栈的深度，等，所以code属性是值code 属性表

下面看一下code 属性表的格式：
```

| 属性名称               | 长度           | 含义                                       |
| ---------------------- | -------------- | ------------------------------------------ |
| attribute_name_index   | u2             | 属性名在常量池中的索引                     |
| attribute_length       | u4             | 标明了属性表接下来内容占据的空间           |
| max_stack              | u2             | 该方法被调用到栈中的栈的最大深度           |
| max_locals             | u2             | 标明了局部变量，形参所在的局部变量表的长度 |
| code_length            | u4             | 字节码指令的条数                           |
| codc                   | u1             | 字节码指令，一共有code_length 个字节码指令 |
| exception_table_length | u2             | 异常表的长度                               |
| exception_table        | exception_info | 异常信息的表                               |
| attributes_count       | u2             | code 属性表的属性表                        |
| attribute_info         | attributes     | attributes_count                           |







> ### attribute_name_index

```java
前面说过，JVM 解析字节码文件中的属性表的时候，因为属性表的属性长度，结构不同，所以需要知道解析的是什么徐行，所以需要attribute_name_index 获取属性名，每个属性结构中都有'attribute_name_index'
```



> ### attribute_length

```json
整个属性表的长度，告诉JVM 接下来属性表内容占据空间的偏移地址，attribute_length-6（name_index(u2),att_length(u4)）
```



> ### max_stack

```json
max_stack 代表操作数栈深度的最大值，在方法执行的任意时刻，操作数栈都不能超过这个深度，JVM有时候需要根据这个值分配栈帧
```



> ### max_locals

```json
代表局部变量表所以的存储空间，最小单位是slot，也就是32位 4 byte,
```





> ### 字节码指令序列，code_length ,code

```json
code_length 代表字节码长度，code用于存储指令，每个指令的长度是u1 类型的 也就是可以对应256条指令，JVM 中其中有200+指令
code_length 最大值是41亿，也就是说一个方法被编译后可以有41亿条指令，但是JVM规定一个方法的指令树不能超过
65535条，也就是说code_length 实际只使用2 byte 的空间
```

