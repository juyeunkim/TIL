# CaseFormat

> `import com.google.common.base.CaseFormat;`



## 종류

- LOWER_HYPHEN : `lower-hyphen`
- LOWER_UNDERSCORE : `lower_underscore`
- LOWER_CAMEL : `lowerCamel`
- UPPER_CAMEL : `UpperCamel`
- UPPER_UNDERSCORE : `UPPER_UNDERSCORE`





## SnakCase -> CamelCase

```java
CaseFormat.LOWER_UNDERSCORE.to(CaseFormat.LOWER_CAMEL, "db_col_nm")
```



## CamelCase -> SnakeCase

~~~java
CaseFormat.LOWER_CAMEL.to(CaseFormat.LOWER_UNDERSCORE, "dbColNm")
~~~



