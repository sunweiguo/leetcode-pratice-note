## 题目描述

Given an array of integers and an integer k, find out whether there are two distinct indices i and j in the array such that nums[i] = nums[j] and the absolute difference between i and j is at most k.

中文题目描述有问题。。。英文题的翻译应该是：「二者差的绝对值不超过 k 即可」，但是题目中的却是「二者差的绝对值最大为 k」。

示例 1:


```
输入: nums = [1,2,3,1], k = 3
输出: true
```

示例 2:


```
输入: nums = [1,0,1,1], k = 1
输出: true
```

示例 3:


```
输入: nums = [1,2,3,1,2,3], k = 2
输出: false
```

示例 4：

```
输入: nums = [99,99],k=2
输出: true
```

## 解题思路

第一个想到的是暴力解法，双层for循环去逐个寻找，一旦找到满足条件的就停止。

```java
class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        //省略非空等判断
        for(int i=0;i<nums.length;i++){
            for(int j=i+1;j<nums.length;j++){
                if(nums[i] == nums[j] && Math.abs(i-j) <= k){
                    return true;
                }
            }
        }        
        return false;
    }
}
```

执行结果不理想：

```
执行用时 :303 ms, 在所有 Java 提交中击败了21.49%的用户
内存消耗 :41.2 MB, 在所有 Java 提交中击败了96.84%的用户
```

其实可以借助map来实现，按照经验，一般的数组查找题目都可以利用map来解决：

```java
import java.util.*;
class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        if(nums.length <= 0 || nums == null){
            return false;
        }
        //map，key存储元素值nums[i]，value存储索引i
        Map<Integer,Integer> map = new HashMap<>();
        for(int i=0;i<nums.length;i++){
            //map中存在说明元素相等，此时判断索引距离是否小于等于k，是则找到了，不是继续努力
            if(map.containsKey(nums[i]) && Math.abs(i-map.get(nums[i]))<=k){
                return true;
            }
            map.put(nums[i],i);
        }
        //走到这一步就说明找不到了
        return false;
    }
}
```

执行效率得到了大幅的提升，虽然用了额外的O(n)的空间，不过空间换时间往往是值得的，也是提升算法效率的一个捷径：

```
执行用时 :12 ms, 在所有 Java 提交中击败了93.50%的用户
内存消耗 :43 MB, 在所有 Java 提交中击败了82.02%的用户
```

不过这个方法是否可以简单点写？我在题解中看到用set来实现的，思路十分简单：

- 遍历数组，对于每个元素做以下操作：
    - 在散列表中搜索当前元素，如果找到了就返回 true。
    - 在散列表中插入当前元素。
    - 如果当前散列表的大小超过了 k， 删除散列表中最旧的元素。

最后一步很关键，只要set的长度大于k了，那么最旧的元素也就失去了去查询的意义，直接去除掉，并且这样做的好处是，控制一个set的窗口大小，查询上只需要对这k个元素查询即可，某种意义上来说提高了一定的查询效率，虽然也不大。最后就是set的实现代码比map的实现代码要简单点^^。

## 提交代码

```java
import java.util.Set;
import java.util.HashSet;
class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        Set<Integer> set = new HashSet<>();
        for(int i=0;i<nums.length;i++){
            if(set.contains(nums[i])){
                return true;
            }
            set.add(nums[i]);
            if(set.size() == k+1){
                set.remove(nums[i-k]);
            }
        }
        return false;
    }
}
```



不知道为什么，提交几遍，这种方式执行用时比map要长。。。。