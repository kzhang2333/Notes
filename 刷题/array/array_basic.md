# What is array?

Array 是定长、定类型、提供O(1) index access 的数据结构

优点：

- O(1) index read、update

缺点

- 不可变长度
- insert、delete by index操作消耗大

# 类似数据结构

ArrayList - 可变长的array

2D array - array of array，可以将2Darray拍扁为1D

String - array of char，也是提供O(1) index access，有时需要把String 转换成char array

LinkedList - 同为1-D数据结构，但是不提供index access

# 常见算法

- binary search：每次都可以排除掉一部分search space
- 指针
  - 单指针scan
  - 双指针嵌套scan：可以获取all combination of 2
  - n指针嵌套scan：比较少见
- map
  - 以array代替hashmap， 一般出现在所有key范围已知且可以map到数字上（如：a-z --> 0 - 25）
  - 建立从value到index的mapping关系
- Sort
  - mergeSort
  - quickSort
  - selectionSort
  - rainbowSort
- n-sum
  - 2-sum：1D scan + mapping from value to index
  - 3-sum：
- sliding-window



# 实战算法题

## 1. Two Sum - LC01

题目要求：Given int array and target, find if there are two nums in array whose sum is target. If so, return indies of two numbers; if not, return -1, -1

费曼方法：想象一个自动售货机，需要给顾客找零。假设自动售货机只有硬币，且硬币面值随机（与现实不同），而且必须找给顾客两枚硬币（不能多也不能少）。

Brute force：双指针找到所有combination，Time O（n^2), SpaceＯ（１）

Use Hashmap：记录出现过的数字以及他们的位置，当我们看到一个新的数字，计算新数字与target的差值（补数），并询问map是否存在

- two pass：先scan一遍，把每一个数字加入map；再scan一遍，scan到一个新数字时检查后面有没有需要的补数
- one pass（better）：一遍scan，scan到一个新数字时检查前面是否出现过这个数字的补数

最佳方法的时空复杂度（one pass hashmap）：

- time： O（n） scan每一个数字，每次检查耗费O(1）
- space：O(n) 可能不存在答案，所以每一个数字都被加入hashmap中

## 1.1 Two Sum Sorted - LC

首尾双指针

和小挪左，和大挪右

利用sorted属性

## 2. Three Sum - LC15

从brute force开始，而不应该从two sum开始

类似的较简单的问题应该成为工具，而不是出发点，brute force永远是出发点

- 找到所有triplets
- deduplication

## 3. Three Sum Closet - LC16

## 4. Four Sum - LC18

































