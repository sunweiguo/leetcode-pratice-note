## 题目描述

给定一个已按照升序排列 的有序数组，找到两个数使得它们相加之和等于目标数。

函数应该返回这两个下标值 index1 和 index2，其中 index1 必须小于 index2。

说明:

返回的下标值（index1 和 index2）不是从零开始的。
你可以假设每个输入只对应唯一的答案，而且你不可以重复使用相同的元素。
示例:


```
输入: numbers = [2, 7, 11, 15], target = 9
输出: [1,2]
解释: 2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2 。
```



## 解题思路

因为是有序数据，那么就比较简单了，一头一尾前后夹逼即可。

## 提交代码


```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        //存放结果
        int[] res = new int[2];
        //定义一头一尾的指针，前后夹逼试探
        int low = 0,high = numbers.length-1;
        while(low < high){
            if(numbers[low] + numbers[high] == target){
                //注意下返回结果是数组下标+1
                res[0] = low + 1;
                res[1] = high + 1;
                return res;
            }else if(numbers[low] + numbers[high] < target){
                low++;
            }else{
                high--;
            }
        }
        return res;
    }
}
```
