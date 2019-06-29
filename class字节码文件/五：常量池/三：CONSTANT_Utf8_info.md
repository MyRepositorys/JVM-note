# 三：CONSTANT_Utf8_info



> ### CONSTANT_Utf8_info 的结构

| 名称   | 类型      | 数量 |
| ------ | --------- | ---- |
| tag    | u1        | 1    |
| length | u2        | 1    |
| bytes  | ${elngth} |      |



```json
CONSTANT_Utf8_info 的tag 值是1，
length 指的是紧跟着length 后的二进制序列的字节长度
bytes 就是指字符串经过UTF-8 变长编码后的二进制字节序列
前面介绍过UTF-8变长编码，每个字符所占的字节并不是固定的，
```

