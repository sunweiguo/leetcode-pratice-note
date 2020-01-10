## 题目描述

给定一个非负索引 k，其中 k ≤ 33，返回杨辉三角的第 k 行。

在杨辉三角中，每个数是它左上方和右上方的数的和。

示例:


```
输入: 3
输出: [1,3,3,1]
```

进阶：

你可以优化你的算法到 O(k) 空间复杂度吗？

## 解题思路


```java
class Solution {
    //注意本题的rowIndex是从0开始计算的
    public List<Integer> getRow(int rowIndex) {
        //存放最终结果的集合
        List<List<Integer>> res = new ArrayList<>();
        //每一行结果存放
        List<Integer> rowList = null;
        //将杨辉三角的结构以二维数组来描述，方便进行三角的构建
        //此时默认数组所有元素都是0
        int[][] arr = new int[rowIndex+1][rowIndex+1];
        //开始构建每一行的数组的数据，并且最终放到list中返回
        for(int i=0;i<=rowIndex;i++){
            rowList = new ArrayList<>();
            //每一行的第一个元素都是1
            arr[i][0] = 1;
            rowList.add(arr[i][0]);
            //开始从每一行的第二个元素计算，如何计算呢？其实就是依靠上一行元素进行计算的，公式为：
            //arr[i][j] = arr[i-1][j-1] + arr[i-1][j]
            for(int j=1;j<=i;j++){
                arr[i][j] = arr[i-1][j-1] + arr[i-1][j];
                rowList.add(arr[i][j]);
            }
            //如果是第rowindex行了，则直接返回即可
            if(i == rowIndex){
                return rowList;
            }
            res.add(rowList);
        }
        //为了程序不会报错，返回集合的最后一个元素
        return res.get(res.size() - 1);
    }
}
```

那么如何达到O(K)的空间复杂度呢？其实没必要用一个数组来存储所有的元素，可以用一个list来存放上一行的数据，见下面代码：


## 提交代码


```
import java.util.*;
class Solution {
    //注意本题的rowIndex是从0开始计算的
    public List<Integer> getRow(int rowIndex) {
        //curr用来存放当前行元素
        List<Integer> curr = null;
        //pre用来存放上一行元素
        List<Integer> pre = null;
        for(int i=0;i<=rowIndex;i++){
            curr = new ArrayList<>();
            for(int j=0;j<=i;j++){
                //根据规律，每一行第一个元素和第i个元素都是为1
                if(j == 0 || j == i){
                    curr.add(1);
                }else{
                    curr.add(pre.get(j-1)+pre.get(j));
                }
            }
            //更新pre指向curr，用于下一行循环，即保存了新的上一行数据用于下一行的计算，节省了空间复杂度
            pre = curr;
        }
        return curr;
    }
}
```

## 结语

杨辉三角作为一个经典题目，在大学学习编程的时候或许就遇到过这个问题，其实还有很多很多的优化方案，希望自己以后能够多扩展思路，不能为了做题而做题，因此，总有一天我会回来的，将这道题目优化到底。