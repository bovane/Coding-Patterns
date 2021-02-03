[TOC]

# Coding Patterns of sliding window

## 应用场景

滑动窗口模式（sliding window pattern）是用于在给定数组或链表的特定窗口大小上执行所需的操作，比如寻找包含所有 1 的最长子数组。从第一个元素开始滑动窗口并逐个元素地向右滑，并根据你所求解的问题调整窗口的长度。在某些情况下窗口大小会保持恒定，在其它情况下窗口大小会增大或减小。

<img src="1-Coding Patterns of sliding window.assets/sliding-window.png" alt="sliding-window" style="zoom:50%;" />

### A example

```c++
Given a array：[a b c d e f g h]
  
a sliding window of size 3 would run over it like：
  
[a b c]
  [b c d]
    [c d e]
      [d e f]
        [e f g]
          [f g h]
```

下面是一些你可以用来确定给定问题可能需要滑动窗口的方法：

- 问题的输入是一种线性数据结构，比如链表、数组或字符串
- 你被要求查找最长/最短的子字符串、子数组或所需的值
- 这儿存在高达$O(n^2)$或$O(n^3)$的时间复杂度的暴力破解方法

你可以使用滑动窗口模式处理的常见问题：

- 大小为 K 的子数组的最大和（简单）
- 带有 K 个不同字符的最长子字符串（中等）
- 寻找字符相同但排序不一样的字符串（困难）

## 原理描述

滑动窗口模式（sliding window pattern）常见于处理子数组、子列表问题。那些用暴力破解方法的问题通常达到$O(n^2)$或$O(n^3)$的时间复杂度，而使用滑动窗口方法可以将时间复杂度降低到$O(n)$。

# 力扣实战

## Maximum Sum Subarray of Size K (easy)

### 问题描述

Given an array nums and a target value k, find the maximum length of a subarray that sums to k. If there isn’t one, return 0 instead

**Note:**
The sum of the entire nums array is guaranteed to fit within the 32-bit signed integer range.

**example**

```java
Given nums = [1,-1,5,-2,3],k = 3,
return 4.(because the subarray [1,-1,5,-2] sums to 3 and is the longest)
          
Given nums = [-2,-1,2,1], k = 1,
return 2.(because the subarray [-1,2] sums to 1 and is the longest)   
```

### 解决方法

该问题为给定K-子数组最大和问题，通过分析知我们立马用暴力破解方法，使用两层循环得到检查所有的和等于K的子数组并求得子数组中最大的子数组，该方法时间复杂度为$O(N^2)$。通过仔细分析可知，该问题符合滑窗法的基本使用规则：输入是线性结构【数组】；要求查找子数组的最大值；存在多层循环的暴力破解方法；因此我们可以想到用滑动窗口法解决该问题。

**借助哈希构造累加和思想：**采用哈希表和累加和来做（累加和：累计和的数组 `sum​`，其中 `sum[i] `​表示 `​[0, i]​`区间的数字之和，那么` [i,j]` 区间之和就可以表示为​ `sum[j]-sum[i-1]​）`我们定义一个哈希表 —— `key -- sum[i]；value -- i`

这样在遍历数组中的数字的时候，假设累积和是`sum`，一旦发现`（sum - k）`已经在哈希表中出现，则说明区间`[hash[sum - k], i]`的和就是`k`，此时就可以更新返回值的大小。

### 代码

**暴力破解法：C++**

```c++
from itertools import accumulate
class Solution:
    def maxSubArrayLen(self, nums, k):
        """
        :type nums: list
        :type k: int
        :rtype: int
        """
        pre_sum = [0]+list(accumulate(nums))
        res = 0
        for i in range(len(pre_sum)):
            for j in range(i, len(pre_sum)):
                if pre_sum[j] - pre_sum[i] == k:
                    res = max(res, j - i)
        return res
                      
/* 时间复杂度：O(N*N) 
	 空间复杂度：O(N)
*/
```

**哈希表法**：C++

```c++
from itertools import accumulate
class Solution:
    def maxSubArrayLen(self, nums, k):
        """
        :type nums: list
        :type k: int
        :rtype: int
        """
        pre_sum = list(accumulate(nums))
        res, dic = 0, dict()

        for i in range(len(pre_sum)):
            if pre_sum[i] == k:
                res = i + 1
            elif pre_sum[i] - k in dic:
                res = max(res, i - dic[pre_sum[i] - k])
            if pre_sum[i] not in dic:
                dic[pre_sum[i]] = i

        return res
/* 时间复杂度：O(N) 
	 空间复杂度：O(N)
*/
```

**哈希表法：Java**

```java
public class Solution {
  /** 
  	type nums: int
  	type k: int
  	rtype: int
  */
  public int maxSubArrayLen(int[] nums, int k) {
    int sum = 0, max = 0;
    HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
    
    for (int i = 0; i < nums.length; i++) {
        sum = sum + nums[i];
        if (sum == k) 
          max = i + 1;
        else if (map.containsKey(sum - k)) 
          max = Math.max(max, i - map.get(sum - k));
        if (!map.containsKey(sum)) 
          map.put(sum, i);
    }
    return max;
	}
/** 
	 时间复杂度：O(N) 
	 空间复杂度：O(N)
*/
}
```

### 课后回顾



## Smallest Subarray with a given sum (easy)

### 问题描述

### 解决方法

### 代码

### 课后回顾

## Longest Substring with K Distinct Characters (medium)

### 问题描述

### 解决方法

### 代码

### 课后回顾

## Fruits into Baskets (medium)

### 问题描述

### 解决方法

### 代码

### 课后回顾

## No-repeat Substring (hard)

### 问题描述

### 解决方法

### 代码

### 问题回顾

## Longest Subarray with Ones after Replacement (hard)

### 问题描述

### 解决方法

### 代码

### 问题回顾

## Longest Substring with Same Letters after Replacement (hard)

### 问题描述

### 解决方法

### 代码

### 问题回顾

## Problem Challenge 1 - Permutation in a String (hard) 

### 问题描述

### 解决方法

### 代码

### 问题回顾

## Problem Challenge 2 - String Anagrams (hard) 

### 问题描述

### 解决方法

### 代码

### 问题回顾

## Problem Challenge 3 - Smallest Window containing Substring (hard)

### 问题描述

### 解决方法

### 代码

### 问题回顾

## Problem Challenge 4 - Words Concatenation (hard) 

### 问题描述

### 解决方法

### 代码

### 问题回顾

# 总结回顾



