---
title: C++ - Data Types
draft: false
date: 2019-12-28
tags: ["c++"]
---

Data type is probably the most fundamental topic pertinent to a programming language.
There is a plethora of online resources explaining all the available data types in C++, and the intend of this post is to serve as a quick reference for my future lookup because I used to check times and again some of the items in this post.
It's time to avoid all that hassles and put them all in one place.

There are two main categories of data types in C++, namely, arithmetic types and a special type named **void**, which will be elaborated separately in following sections.

## Arithmetic Types

There exist four sub-categories within arithmetic types:

+ boolean
  + **bool**: can only take values of *true* or *false*.
+ character
  + **char**: represent a character. There are three other types for representing extended characters: *wchar_t*, *char16_t* and *char32_t*.
        + char, signed char, unsigned char
+ integral types
  + **short**
        + signed short, unsigned short
  + **int**
        + signed int, unsigned int
  + **long**
        + signed long, unsigned long
  + **long long**
        + signed long long, unsigned long long
+ floating-point types
  + **float**
        + signed float, unsigned float
  + **double**
        + signed double, unsigned double
  + **long double**
        + signed long double, unsigned long double

### Signed and unsigned

Except for *bool* and extended character types, arithmetic types can be either signed or unsigned.
As the name implies, a signed type can represent both positive and negative values, whereas an unsigned type can only represent nonnegative values.

**Caution**: there are actually three character types: *char*, *signed char* and *unsigned char*. A char is not necessarily signed char - whether it's signed or unsigned depends on the compiler!

**Caution**: signed values are implicitly converted into unsigned values if they show up in the same expression!

### Type conversion

Type conversion can happen intentionally or automatically, and is the culprit of many programming errors.
Below is a few common notes:

+ Assigning a non*bool* arithmetic types to a bool object will result as *false* if the value is 0, and *true* otherwise.

### Literals

#### Character and string literal

String literal takes one more space that its apparent size as the compiler appends a null character (’\0’) to every string literal.

#### Boolean literal

*true* and *false* are boolean literals.

#### Pointer literal

*nullptr* is a pointer literal.

### Min and max values

C++ defines a template class *numeric_limits* to check the minimum and maximum values of a data type.

```cpp
#include <iostream> // std::cout
#include <limits> // std::numeric_limits

using namespace std;

int main(int argc, const char * argv[]) {

    cout << "bool: min = " << numeric_limits<bool>::min() 
         << "\tmax = " << numeric_limits<bool>::max() << endl;

    cout << "char: min = " << (int)numeric_limits<signed char>::min() 
         << "\tmax = " << (int)numeric_limits<signed char>::max() << endl;

    cout << "int: min = " << numeric_limits<int>::min() 
         << "\tmax = " << numeric_limits<int>::max() << endl;

    cout << "double: min = " << numeric_limits<double>::min() 
         << "\tmax = " << numeric_limits<double>::max() << endl;

    return 0;
}
```

## Void Type

The *void* serves multiple purposes:

+ function return type
+ function parameter data type
+ pointer type
  + A pointer of type *void\** can point to any variable not declared with const or volatile, and cannot be dereferenced until cast into another type.
  + Cannot declare a variable of type *void*.
