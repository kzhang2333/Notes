# Java Basics - char, String, Unicode, ASCII
## char - primitive type
1. 16 bits, 2 bytes
2. can do basic operation

## String - a array of chars
1. Object
2. Immutable

### char是如何储存的
原理：一个字典，每页有一个字，电脑里储存的是这个字符对应的页码。所以每一个字符在电脑中以一个整数的
形式储存。那么我们怎么知道哪个数字对应哪个字符呢

有两个标准:
#### ASCII - Old, Small Dictionary
> '0' = 48
> 'A' = 65
> 'a' = 97
注意，ASCII 包含两部分，0-127是基本ASCII码，主要是常用英文字符；128-255 是extended ASCII code，包含了一些不常用
字符。
#### Unicode - Big, New Dictionary
我们之所以可以用ASCII chart是因为在unicode里的前256位留给了ASCII，所以可以直接对应。
> u+16xx =

### How Java Store char
Java use **Unicode**, but in Unicode first 0-127 number are same with ASCII chart to make things
simple. So we can use Chart of ASCII when work with first 128 chars.

### char operation in Java
```
char a = 'a';
char b = 'b';
char c = (char) a + b;
c += a + b;
```
in java，两个不同类型的数据做运算，结果会自动转为较大的那个，比如int + char结果是int。
所以必须进行强制类型转换。在使用`+=`这类运算符时，不必强制类型转换。
