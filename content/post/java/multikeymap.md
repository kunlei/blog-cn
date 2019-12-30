---
title: MultiKeyMap in Java
date: 2019-10-01
draft: false
tags: ["java"]
---

I am working on a project that requires value lookup using multiple keys.
I believe this operation is common in other applications and want to leave a note here.
It turns out that there is already a good open source solution for this purpose and all we need to do is adding the following package as one of the project dependencies through maven:

```java
<!-- https://mvnrepository.com/artifact/org.apache.commons/commons-collections4 -->
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-collections4</artifactId>
    <version>4.0</version>
</dependency>
```

Here is a very simple example showing its usage of two keys (1, 2) and their corresponding value 1.
```java
MultiKeyMap map = new MultiKeyMap();
map.put(1, 2, 1);
Object value = map.get(1, 2);
System.out.println(value);
```
