## 题目描述

对应leetcode的题号为532。

给定一个整数数组和一个整数 k, 你需要在数组里找到不同的 k-diff 数对。这里将 k-diff 数对定义为一个整数对 (i, j), 其中 i 和 j 都是数组中的数字，且两数之差的绝对值是 k.

示例 1:


```
输入: [3, 1, 4, 1, 5], k = 2
输出: 2
解释: 数组中有两个 2-diff 数对, (1, 3) 和 (3, 5)。
尽管数组中有两个1，但我们只应返回不同的数对的数量。
```

示例 2:


```
输入:[1, 2, 3, 4, 5], k = 1
输出: 4
解释: 数组中有四个 1-diff 数对, (1, 2), (2, 3), (3, 4) 和 (4, 5)。
```

示例 3:


```
输入: [1, 3, 1, 5, 4], k = 0
输出: 1
解释: 数组中只有一个 0-diff 数对，(1, 1)。
```

注意:

- 数对 (i, j) 和数对 (j, i) 被算作同一数对。
- 数组的长度不超过10,000。
- 所有输入的整数的范围在 [-1e7, 1e7]。


## 解题思路

这一题可以用两数之和的方法来做，比如当前数位i，那么我只要找到i+k的数字即可（这里好好想下要不要考虑i-k的情况 ^^，其实这里用加k就是巧妙地避开了重复性问题和相减可能是负数等问题）。具体见代码。


不过还有一种思路是先对数组进行排序，然后用两个指针去逐个寻找，利用与k的差值不停地移动左右两个指针。此方法实现上略显繁琐了，因为需要考虑连续重复数字的情况。代码直接从评论区复制而来。

## 提交代码


```java
class Solution {
    public int findPairs(int[] nums, int k) {
        //测试用例竟然出现了k=-1的情况...
        if(k < 0){
            return 0;
        }
        //将所有的数字以及出现的次数保存到一个map中
        Map<Integer,Integer> map = new HashMap<>();
        for(int i=0;i<nums.length;i++){
            if(map.get(nums[i]) == null){
                map.put(nums[i],1);
            }else{
                map.put(nums[i],map.get(nums[i])+1);
            }
        }
        //与两数之和题目的思路一样，逐个去找相差k的数，为了避免重复，只需要找比自己大k的数字即可
        //这里需要特殊处理下k=0的情况，k=0说明需要找重复的数字有几对，那么就是找map中出现次数大于1的数字个数即可
        int count = 0;
        for(int index : map.keySet()){
            if(k == 0){
                if(map.get(index) > 1){
                    count++;
                }
            }else if(map.get(index+k) != null){
                count++;
            }
        }
        return count;
    }
}
```

空间上利用了一个map，因此空间复杂度为O(N)，时间上相当于遍历了两次数组，因此时间复杂度为O(N)。


排序+双指针的做法，是一个很棒的思路，加了点注释：

```java
class Solution {
    public int findPairs(int[] nums, int k) {
        int len = nums.length;
        //先对数组排序
        Arrays.sort(nums);
        //左右指针，分别从0和1开始出发遍历数组
        int left = 0;
        int right = 1;
        int count = 0;
        //每次都要保证right在left的右边，因此边界上只需要考虑right是否出界即可
        while (right < len) {
            //计算差值与k进行比较，小于k那么right加一，大于k那么left加一，相等则同时加一并计数加一
            int t = nums[right] - nums[left];
            if (t < k) {
                right++;
            } else if (t > k) {
                left++;
            } else {
                count++;
                right++;
                left++;
            }
            //排除连续相等的重复元素
            while (right < len && nums[right] == nums[right - 1]) {
                right++;
            }
            while (left > 0 && left < len && nums[left] == nums[left - 1]) {
                left++;
            }
            //始终保证right在left的右边
            if (right <= left) {
                right = left + 1;
            }
        }
        return count;
    }
}
```

由于存在排序，时间复杂度至少是O(NlogN)级别。