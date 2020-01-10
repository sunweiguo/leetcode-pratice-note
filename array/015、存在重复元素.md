## 题目描述

给定一个整数数组，判断是否存在重复元素。

如果任何值在数组中出现至少两次，函数返回 true。如果数组中每个元素都不相同，则返回 false。

示例 1:


```
输入: [1,2,3,1]
输出: true
```

示例 2:

```
输入: [1,2,3,4]
输出: false
```

示例 3:


```
输入: [1,1,1,3,3,4,3,2,4,2]
输出: true
```


## 解题思路

寻找重复元素的思路，比暴力解法还直接进入我脑海的思路还是Map：

```java
import java.util.*;
class Solution {
    public boolean containsDuplicate(int[] nums) {
        if(nums == null || nums.length == 0){
            return false;
        }
        Map<Integer,Integer> map = new HashMap<>();
        for(int i=0;i<nums.length;i++){
            if(map.containsKey(nums[i])){
                return true;
            }else{
                map.put(nums[i],1);
            }
        }
        return false;
    }
}
```

不过执行效率并不满意：

```
执行用时 :14 ms, 在所有 Java 提交中击败了43.49%的用户
内存消耗 :43.6 MB, 在所有 Java 提交中击败了85.01%的用户
```

不过这道题目也没想出其他什么快速的思路，翻了下评论和题解区，也都大同小异，不过这题写法上可以用hashset来精简下，不过看过java容器的同学一定直到hashset的本质就是hashmap.


## 提交代码


```java
import java.util.*;
class Solution {
    public boolean containsDuplicate(int[] nums) {
        if(nums == null || nums.length == 0){
            return false;
        }
        Set<Integer> set = new HashSet<Integer>();
        for(int i=0;i<nums.length;i++){
            if(set.contains(nums[i])){
                return true;
            }else{
                set.add(nums[i]);
            }
        }
        return false;
    }
}
```




