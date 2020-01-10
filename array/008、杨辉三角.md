## 题目描述
给定一个非负整数 numRows，生成杨辉三角的前 numRows 行。

在杨辉三角中，每个数是它左上方和右上方的数的和。

示例:


```
输入: 5
输出:
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```


## 解题思路

见注释。

## 提交代码

```java
/* 1
*  1 1
*  1 2 1
*  1 3 3 1
*  1 4 6 4 1
*  1 5 10 10 5 1
*/
import java.util.*;
class Solution {
    public List<List<Integer>> generate(int numRows) {
        //存放最终结果的集合
        List<List<Integer>> res = new ArrayList<>();
        //每一行结果存放
        List<Integer> rowList = null;
        //将杨辉三角的结构以二维数组来描述，方便进行三角的构建
        //此时默认数组所有元素都是0
        int[][] arr = new int[numRows][numRows];
        //开始构建每一行的数组的数据，并且最终放到list中返回
        for(int i=0;i<numRows;i++){
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
            //走到这每一行的数据已经处理完毕，添加到最终的集合中即可
            res.add(rowList);
        }
        return res;
    }
}
```
