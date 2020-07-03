# What is array?

Array 是定长、定类型、提供O(1) index access 的数据结构

优点：

- O(1) index read、update

缺点

- 不可变长度
- insert、delete by index操作消耗大:O(n) 需要顺延copy更改位置之后的elements

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



# n-sum

## 1. Two Sum - LC01

题目要求：Given int array and target, find if there are two nums in array whose sum is target. If so, return indies of two numbers; if not, return -1, -1

费曼方法：想象一个自动售货机，需要给顾客找零。假设自动售货机只有硬币，且硬币面值随机（与现实不同），而且必须找给顾客两枚硬币（不能多也不能少）。

Brute force：双指针找到所有combination，Time O（n^2), SpaceＯ（１）

Use Hashmap：记录出现过的数字以及他们的位置，当我们看到一个新的数字，计算新数字与target的差值（补数），并询问map是否存在

- two pass：先scan一遍，把每一个数字加入map；再scan一遍，scan到一个新数字时检查后面有没有需要的补数
- one pass（better）：一遍scan，scan到一个新数字时检查前面是否出现过这个数字的补数

hashmap法时空复杂度（one pass hashmap）：

- time： O（n） scan每一个数字，每次检查耗费O(1）
- space：O(n) 可能不存在答案，所以每一个数字都被加入hashmap中

代码：

此处只写hashmap法

```java
public int[] towSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement)) return new int[]{map.get(complement), i};
        map.put(nums[i], i);
    }
    return new int[]{-1, -1};
}
```



## 1.1 Two Sum Sorted - LC167

题目要求：与two sum一致，但是given array is sorted in ascending order

思路：可以使用1.0中的hashmap回头看法，但是无法利用sorted属性，而且空间复杂度较高，这里我们使用two sum及其扩展的另一块基石：two pointers with sorted array

- 双指针，left and right，left 从左边第一个开始，right从右边第一个开始
- 计算左右指针指向数字之和，**和小挪左，和大挪右**
- 循环条件：left < right

proof：

1. Assume we have a sorted array [. . ., a, b, ....., c, d, .....], assume there exists answer [a, d] 
2. Because left pointer starts from left-most index and right pointer starts form right-most, so left or right pointers must meet a or d first. 
3. Assume left pointer meets a first, now right pointer is on right side of d, which means sum of two pointers must be larger than target. 
4. According to our algorithm, left pointer will wait here and right pointer will be moved left until sum is equal or less than target. 
5. If right pointer meets b first, right pointer will wait here and left pointer will be moved to right until sum is equal or larger than target. 

==Proved!==

Complexity:

- Time: O(n)  - one pass scan array
- Space: O(1)

总结：

这是two sum的另一种基础解法，在unsorted array情况下，这种解法不如hashmap，因为sorting最小时间复杂度也要nlogn。但是在follow-up的k-sum系列：three-sum，three-sum closest，four-sum中，由于数据量的增大，时间复杂度至少也是 O（n^k-1), 只要 k > 2, sorting耗费的nlogn不足以影响时间复杂度（至少也是n^2），除此之外，使用two pointers 还有别的好处：

- 可以去重
- 可以做three-sum closest

代码

```java
public static int[] twoSum(int[] numbers, int target) {
    int left = 0;
    int right = numbers.length - 1;
    while (left < right) {
        double sum = (double)numbers[left] + (double)numbers[right]; // 使用double防止内存溢出
        if (sum == target) {
            return new int[]{left + 1, right + 1};
        } else if (sum < target) {
            left++;
        } else {
            right--;
        }
    }
    return new int[]{-1, -1};
}
```

## 2. Three Sum - LC15

题意：找到given array中所有和为0的一组三个数字，return**所有**结果，**并且不能重复**

思路：此题有两个难点，

- 找到所有triplets - 找到所有可能结果
  - brute force：for for for loops, n^3
  - for loop + two sum，two sum有两个方法，这里使用two pointers，因为
- deduplication。去重有两个思路：
  - 一个是把所有已知结果存入hashset，发现新结果时检查hashset，这样就必须保证三个数字以一定顺序存入hashset，这里记录三个数中最大和最小值，并且从小到大放入pair中。这种方法比较复杂
  - 另一个去重的常用方法是sorting，这里使用sorting，正如前面所说，当k大于2， sorting的时间不影响时间复杂度

> 必须从brute force开始，而不应该从two sum开始。类似的较简单的问题应该成为工具，而不是出发点，brute force永远是出发点

## 3. Three Sum Closet - LC16

类似于Three Sum，但是这里必须使用two pointer，因为寻找的是最近的，hashmap只能找到excetly same one

## 4. Four Sum - LC18

- brute force：for for for for loop + 去重
- for for loop + two sum + 去重

## 5. k-sum

此类题有的基本思路：

- brute force：k for loop找到所有的k 个nums组成的set，并且利用sorting去重，每一道题都必须从brute force出发，这样可以不用考虑之前的数字
- better: k - 1 for loop + two sum. two sum有两种方法：
  - hashmap
  - two pointer：只要k > 2，都使用two pointer
- 所以使用复杂度
  - time: O(n ^ k - 1)
  - space: O(logn) - O(n) 取决于sorting algorithm

# Two pointer

## 1. Container with most water - LC 11

题意：Given *n* non-negative integers *a1*, *a2*, ..., *an* , where each represents a point at coordinate (*i*, *ai*). *n* vertical lines are drawn such that the two endpoints of line *i* is at (*i*, *ai*) and (*i*, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

理解：

- 注意这里的每一个bar是没有宽度的，把每一个bar想象成没有厚度的墙
- 每两堵墙构成一个container，且储水能力由较矮的墙决定

Brute Force - Enumeration

- enumerate all two number pairs
- for for loop 
- Keep track of max water 

Two pointer:

- 左指针从0开始，右指针从length - 1开始
- 每一对左右指针形成一个container，与公共最大出水量对比
- 讲较低的那一面墙的指针向内移动一格

code

```java
// two pointers
// or GIVE UP no useful line
public int maxArea(int[] height) {
    // assume n is at least 2
    int max = Integer.MIN_VALUE;
    int left = 0;
    int right = height.length - 1;
    while (left < right) {
        int area = Math.min(height[left], height[right]) * (right - left);
        max = Math.max(max, area);
        if (height[left] < height[right]) {
            left++;
        } else {
            right--;
        }
     return max;
}
```

==proof==

```
Proved by contradiction:

Suppose the returned result is not the optimal solution. Then there must exist an optimal solution, say a container with a_ol and a_or (left and right respectively), such that it has a greater volume than the one we got. Since our algorithm stops only if the two pointers meet. So, we must have visited one of them but not the other. WLOG, let's say we visited a_ol but not a_or. When a pointer stops at a_ol, it won't move until

The other pointer also points to a_ol.
In this case, iteration ends. But the other pointer must have visited a_or on its way from right end to a_ol. Contradiction to our assumption that we didn't visit a_or.

The other pointer arrives at a value, say a_rr, that is greater than a_ol before it reaches a_or.
In this case, we does move a_ol. But notice that the volume of a_ol and a_rr is already greater than a_ol and a_or (as it is wider and heigher), which means that a_ol and a_or is not the optimal solution -- Contradiction!
```

[证明](https://leetcode.com/problems/container-with-most-water/discuss/6089/Anyone-who-has-a-O(N)-algorithm/7268)

## 2. Trapping Rain Water- LC42

Brute force: find left max and right max for each bar

```java
// brute force: for each bar, find left_high and right_high
public int trapFindLeft_highRight_high(int[] height) {
    int total = 0;
    for (int i = 0; i < height.length; i++) {
        int left_high = 0, right_high = 0;
        for (int j = i - 1; j >= 0; j--) {
            left_high = Math.max(left_high, height[j]);
        }
        for (int k = i + 1; k < height.length; k++) {
            right_high = Math.max(right_high, height[k]);
        }
        int water = Math.min(left_high, right_high) - height[i];
        total += Math.max(0, water);
    }
    return total;
}
```



DP with space O(n)

```java
// better solution:
// We iterate left and right parts again and again. We can store them - OR Dynamic Programming
public int trapDP(int[] height) {
    int[] left_max = new int[height.length];
    int[] right_max = new int[height.length];
    for (int i = 1; i < height.length; i++) {
        left_max[i] = Math.max(left_max[i - 1], height[i - 1]);
    }
    for (int i = height.length - 2; i >= 0; i--) {
        right_max[i] = Math.max(right_max[i + 1], height[i + 1]);
    }
    int total = 0;
    for (int i = 0; i < height.length; i++) {
        int water = Math.min(left_max[i], right_max[i]) - height[i];
        total += Math.max(0, water);
    }
    return total;
}
```



**Two pointer** one pass 牛逼写法

```java
// two pointers 
// 谁小挪谁
public int trap(int[] height) {
    int left = 0, right = height.length - 1;
    int left_max = 0, right_max = 0;
    int total = 0;
    while (left <= right) { // 这里高度关注
        if (left_max < right_max) {
            total += Math.max(0, left_max - height[left]);
            left_max = Math.max(left_max, height[left++]);
        } else {
            total += Math.max(0, right_max - height[right]);
            right_max = Math.max(right_max, height[right--]);
        }
    }
    return total;
}
```

高度关注while loop的循环条件什么是等于？left和right指针之间代表的是什么

# Best Time Sell/Buy Stocks

https://www.youtube.com/watch?v=gsL3T9bI1RQ

# Maximun subarray

```java
class Solution {
    // brute force
    // enumerate all possible subarray
    public int maxSubArrayEnumerate(int[] nums) {
        int maxSum = Integer.MIN_VALUE;
        for (int i = 0; i < nums.length; i++) {
            int sum = 0;
            for (int j = i; j < nums.length; j++) {
                sum += nums[j];
                maxSum = Math.max(maxSum, sum);
            }
        }
        return maxSum;
    }
    
    // DP - O(n) space
    public int maxSubArrayNSpace(int[] nums) {
        if (nums == null || nums.length == 0) return 0;
        // define int[] 
        int[] maxSum = new int[nums.length];
        // maxSum[i] means sum of largest subarray ended and included nums[i]
        int globalMax = nums[0];
        maxSum[0] = nums[0];
        for (int i = 1; i < nums.length; i++) {
            maxSum[i] = Math.max(nums[i], nums[i] + maxSum[i - 1]);
            globalMax = Math.max(globalMax, maxSum[i]);
        }
        return globalMax;
    }
    
    // DP - O(1) space
    public int maxSubArray(int[] nums) {
        if (nums.length == 0) return 0;
        int globalMax = nums[0];
        int maxSum = nums[0];
        for (int i = 1; i < nums.length; i++) {
            maxSum = maxSum > 0 ? nums[i] + maxSum : nums[i];
            globalMax = Math.max(globalMax, maxSum);
        }
        return globalMax;
    }
    
    // divide and conquer - stupid 
    
}
```

如果写divide and conquer，分为左subarray和右subarray，分别求解左边最大subarray，右边最大subarray，以及同时包含左右的最大subarray

































