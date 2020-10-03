# 46-60

## 46. 把数字翻译成字符串

题设：给定一个数字，我们按照如下规则把它翻译为字符串：0 翻译成 “a” ，1 翻译成 “b”，……，11 翻译成 “l”，……，25 翻译成 “z”。一个数字可能有多个翻译。请编程实现一个函数，用来计算一个数字有多少种不同的翻译方法。

示例 1:

输入: 12258
输出: 5

思路：dp[i]为以i结尾的翻译方法数。dp[0] = 0。

- dp[i]单独翻译必成功，dp[i] = dp[i - 1]
- 和前一个一起翻译，若成功，dp[i] += dp[i - 2]

```java
class Solution {
    public int translateNum(int num) {
        String string = String.valueOf(num);
        int[] dp = new int[string.length() + 1];
        dp[0] = 1;
        dp[1] = 1;
        for (int i = 2; i <= string.length(); i++) {
            dp[i] = dp[i - 1];
            if (string.charAt(i - 2) != '0') {
                int bef = Integer.parseInt(String.valueOf(string.charAt(i - 2)) + String.valueOf(string.charAt(i - 1)));
                if (bef < 26) {
                    dp[i] += dp[i - 2];
                }
            }
        }
        return dp[string.length()];
    }
}
```

## 49. 丑数

题设：我们把只包含质因子 2、3 和 5 的数称作丑数（Ugly Number）。求按从小到大的顺序的第 n 个丑数。

示例:

输入: n = 10
输出: 12
解释: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 是前 10 个丑数。
说明:  

1 是丑数。
n 不超过1690。

思路：所有丑数都由这三个因子生成。核心思想：假设丑数序列为1，2，3，4，5，。。。。。则一定是由

```java
nums2 = {1*2, 2*2, 3*2, 4*2, 5*2, 6*2, 8*2...}
nums3 = {1*3, 2*3, 3*3, 4*3, 5*3, 6*3, 8*3...}
nums5 = {1*5, 2*5, 3*5, 4*5, 5*5, 6*5, 8*5...}
# 注意 7 不是丑数. 
# 2, 3, 5 这前 3 个丑数一定要乘以其它的丑数， 所得的结果才是新的丑数， 所以上例中没有出现 7*2, 7*3, 7*5
```

这三个数组有序合并而成。说白了，就是把所有丑数列出来，然后从小到大排序。而大的丑数必然是小丑数的2/3/5倍，所以有了那3个数组。每次就从那数组中取出一个最小的丑数归并到目标数组中。

```java
class Solution {
    public int nthUglyNumber(int n) {
        int[] dp = new int[n];
        dp[0] = 1;
        int d2 = 0, d3 = 0, d5 = 0;
        for (int i = 1; i < n; i++) {
            dp[i] = Math.min(dp[d2] * 2, Math.min(dp[d3] * 3, dp[d5] * 5));
            /*
            ++的原因是，每次判断成功之后该序列向后推进一位。
             */
            if (dp[i] == dp[d2] * 2) {
                d2++;
            }
            if (dp[i] == dp[d3] * 3) {
                d3++;
            }
            if (dp[i] == dp[d5] * 5) {
                d5++;
            }
        }
        return dp[n - 1];
    }
}
```

## 53 - II. 0～n-1中缺失的数字

