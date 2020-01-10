## 题目描述

给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

示例:


```
输入: [-2,1,-3,4,-1,2,1,-5,4],
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```



## 解题思路

这道题目可以利用贪心算法的思想来解决，时间复杂度为O(n)，所谓贪心算法是指，在对问题求解时，总是做出在当前看来是最好的选择。也就是说，不从整体最优上加以考虑，它所做出的仅仅是在某种意义上的局部最优解。

那么我们可以从头开始往后试，定义一个值叫sum，这个sum专门来计算连续子数组的和，因为贪心嘛，追求的是最好每次sum都在逐渐增加，但是呢，实际上我又不能每次都管增加的，有的时候会适当下降是为了下一个元素的猛增。因此其实也不贪心，标准设置为0，只要sum不小于0我就一直往后加，一旦小于0，那么此时sum包含的子数组串已经失去意义了，就从新的位置重新计算sum。在这过程中，一直与最大值做比较，从而在局部的最优解中逐渐获取到全局最优解。代码如下：


```java
class Solution {
    public int maxSubArray(int[] nums) {
        int res = nums[0];
        int sum = nums[0];
        for(int i=1;i<nums.length;i++){
            if(sum > 0){
                //收益是正的，则加上当前值再去试试
                sum += nums[i];
            }else{
                //如果sum加上当前的值都小于0了，干脆sum就改为当前值再继续往后试
                sum = nums[i];
            }
            //res记录的就是最大的和
            if(sum > res){
                res = sum;
            }
        }
        return res;
    }
}
```

此题其实是动态规划的典型题目（上面个代码是用贪心角度来说的，其实吧，跟下面的动态规划也没啥大区别，不过姑且分开吧，因为动态规划是有其强烈的自身标识的，即可以用一个表达式来表达出求解规律）


```
dp[i] 定义为数组nums 中已num[i] 结尾的最大连续子串和， 则有：
dp[i] = max(dp[i-1] + nums[i], num[i]);
```

其实就是说，【前面比较的结果+当前值】与【当前值】做比较，谁大就取谁。其实跟上面所谓的贪心思路是不是差不多？实际上，这种看起来思路是清晰一点的，掌握了动态规划还是可以写出来的，不过上面的贪心是需要一定的功力才能写出来（我觉得）。

用一个临时数组来存放遍历过程中的最大值，最后取这个临时数组最大值即可，时间复杂度O(n)，空间复杂度O(n)：

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int[] dp = new int[nums.length];
        //给dp数组的第一个元素也赋予上值，dp后续的元素就是存放的当前遍历到的最大值
        dp[0] = nums[0];
        int max = nums[0];
        for(int i=1;i<nums.length;i++){
            //动态规划的核心思想：要么取当前值，要么就取以前的最优结果+当前值
            //其实就是看前面算出来的最大值跟当前值结合是否能增大收益
            dp[i] = Math.max(dp[i-1]+nums[i],nums[i]);
            if(dp[i] > max){
                max = dp[i];
            }
        }
        return max;
    }
}
```

其实不需要数组，因为我们可以发现，我们每次只关心dp数组的最后一个有效值，因此我们想办法用一个变量把最后一个有效值保存下来即可。见最终提交代码：

## 提交代码

```java
class Solution {
    public int maxSubArray(int[] nums) {
        //存放最大和
        int max = nums[0];
        //用temp来代替数组，减少空间复杂度
        int temp = nums[0];
        for(int i=1;i<nums.length;i++){
            //a存放的就是之前的dp[i-1]+nums[i]
            int a = temp + nums[i];
            //b存放的就是nums[i]
            int b = nums[i];
            //maxTemp记录此次比较的最大值
            int maxTemp = 0;
            //刷新temp的值，如果a大，那么temp刷新为dp[i-1]+nums[i]
            //如果b大，那么temp刷新为nums[i]，temp就相当于dp[i]里面的值，只是我们不关心i之前的值了，所以只要存下dp[i]的值就够了
            if(a > b){
                maxTemp = a;
                temp = a;
            }else{
                maxTemp = b;
                temp = nums[i];
            }
            //刷新max的值，使得max每次都保存最大值
            if(maxTemp > max){
                max = maxTemp;
            }
        } 
        return max;
    }
}
```
