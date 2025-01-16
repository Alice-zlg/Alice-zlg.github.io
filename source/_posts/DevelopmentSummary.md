---
title: "我的开发总结"
date: 2025-1-16 15:11:00
tag: "java"
---

# JSONObject的使用

## JSONObject 是一种对象，可以存储多种数据类型
它提供了多种方法用于获取存入对象中的数据，其中put方法可以用于单个字符的存储，采用键值对的形式
```Java
JSONObject jsonObject = new JSONObject();

jsonObject.put("abc","abc");

jsonObject.get("abc");
```

其中alibaba提供的JSONObject 类有各种封装方法 使用需要导入对应的依赖
```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.83</version>
</dependency>
```

它不仅支持上面的简单存储，还支持特定的类型的获取，当保存的数据满足获取类型的转换时可以将数据通过特定数据取出
如：
```java
jsonObject.put("abc","85");

jsonObject.put("date","2024-05-6 18:15:45");

jsonObject.getInteger("abc"); // 85

jsonObject.getLong("abc"); // 85

jsonObject.getBigDecimal("abc"); // 85

jsonObject.getDate("date"); // Mon May 06 18:15:45 CST 2024

```
当存入的数据不能转换时就会产生报错
```java
jsonObject.put("date","2024-05-6 18:15:85");

jsonObject.put("date","2024-05-6 18:15");

can not cast to Date, value : 2024-05-6 18:15:85
```
