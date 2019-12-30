---
title: Java Package Lombok
date: 2019-08-23
toc: true
tags: ["java"]
---

This is probably one of those 'everyone knows except me' cases in Java world.
I recently found a new package called 'lombok' that taught me a fresh lesson about how powerful Java annotations could be.
There is something depressing with my short tenure of extensively using Java in daily works.
For often times I find myself doing repetitive works, writing getter/setter methods being one of them (toString(), I am not forgetting you).
It is true that some IDEs, IntelliJ as an example, provides convenient features to automatically generate getters and setter.
Still, creating getters/getters require a few clicks or keyboard presses, and this process is at best boring, and at worst productivity-killing.
Most importantly, those methods must be visually present in the class file, which makes key functions hard to find, if a class happens to have many methods, a situation not rare to see from codes developed by beginners.

I never imagined a Java world before without getters and setters in classes, but apparently someone else did and went ahead inventing it already.
The magic is adding the below Maven dependency and you are free from writing, even seeing, getters and setters!

```xml
<dependencies>
  <dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.8</version>
    <scope>provided</scope>
  </dependency>
</dependencies>
```

It is actually more than that!
This package provides some other annotations that literally free us from many arduous works.
Here is the link to the full list of [available features](https://projectlombok.org/features/all):
```
https://projectlombok.org/features/all
```

Below is an example Java class with some of the most frequently used annotations.
See how concise it is, and now start enjoying a world in which writing getters and setters is as hard as breathing!

```java
import lombok.NoArgsConstructor;
import lombok.AllArgsConstructor;
import lombok.EqualsAndHashCode;
import lombok.Getter;
import lombok.Setter;
import lombok.ToString;

@NoArgsConstructor
@AllArgsConstructor
@Getter
@Setter
@ToString
@EqualsAndHashCode
public class Person {
    private String name;
    private int age;
}
```
