# 二：CONSTANT_Class_info



> ### CONSTANT_Class_info 的结构

| 名称       | 类型 | 数量 |
| ---------- | ---- | ---- |
| tag        | u1   | 1    |
| name_index | u2   | 1    |

```json
'CONSTANT_Class_info' 的tag值是7，
name_index的数据类型和'contant_pool_count'的数据长度是一样的这样做的原因是CONSTANT_Class_info的name_index会索引常量池中的其他项，所以name_index的取值范围和常量池计数值的范围是一样的

CONSTANT_Class_info 几乎索引的都是常量池中的CONSTANT_Utf8_info，
为什么要索引CONSTANT_Utf8_info呢？ 因为CONSTANT_Class_info的本意就是类或接口的符号引用，所谓的符号引用就是类或接口的全限定名，全限定名是字符串，而CONSTANT_Utf8_info是存储字符串的，所以才要索引到CONSTANT_Utf8_info
```

