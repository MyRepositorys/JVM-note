# 三 : version 版本



> ### 版本号存在的意义

```java
JDK 是不断的更新，升级的，所以不同的JDK，编译后的字节码文件的格式是不同的，所以说不通的版本的字节码文件，有不同的数据类型，
版本号的另一个作用是，当JVM加载该字节码文件的时候，如果版本号不在JVM的处理范围内会抛出一个versionException

版本号分为 主版本号（major version）(u1)，次版本号(minor version)(u1), 版本号一共占据 2 byte 其中次版本号排列在主版本号前面，
```

