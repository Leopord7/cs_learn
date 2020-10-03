# 31-45

## 33. 二叉搜索树的后续遍历序列

题设：输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回 `true`，否则返回 `false`。假设输入的数组的任意两个数字都互不相同。

思路：从左开始找到第一个不小于根的结点，分为左右子树，左子树必须全小于根，右子树必须全大于根，再递归判断左右子树。

```java
class Solution {

    public static boolean judge(int[] postorder, int start, int end) {
        if (start >= end) {
            return true;
        }
        int left = start, right = end - 1;
        while (left <= end) {
            if (postorder[left] >= postorder[end]) {
                break;
            }
            left++;
        }
        left--;
        for (int i = left + 1; i <= end - 1; i++) {
            if (postorder[i] <= postorder[end]) {
                return false;
            }
        }
        return judge(postorder, start, left) && judge(postorder, left + 1, end - 1);
    }

    public boolean verifyPostorder(int[] postorder) {
        return judge(postorder, 0, postorder.length - 1);
    }
}
```

## 39. 数组中出现次数超过一半的数字

题设：数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

思路：摩尔投票法。从头开始遍历并每次都假设当前遍历到的为多数元素，其票数为1，其它都为-1。若中途出现票数和为0，重新选新目标。

```java
class Solution {
    public int majorityElement(int[] nums) {
        int x = 0, votes = 0, count = 0;
        for(int num : nums){
            if(votes == 0) x = num;
            votes += num == x ? 1 : -1;
        }
        // 验证 x 是否为众数
        for(int num : nums)
            if(num == x) count++;
        return count > nums.length / 2 ? x : 0; // 当无众数时返回 0
    }
}
```

## 41. 数据流中的中位数

题设：如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。

例如，

[2,3,4] 的中位数是 3

[2,3] 的中位数是 (2 + 3) / 2 = 2.5

设计一个支持以下两种操作的数据结构：

void addNum(int num) - 从数据流中添加一个整数到数据结构中。
double findMedian() - 返回目前所有元素的中位数。

思路：维护两个大小不超过一的堆，大顶堆与小顶堆。前者存小一半后者存大一半。

- 大小相同，进大根堆，删堆顶进小根堆。
- 小根堆多一，进小根堆，删堆顶进大根堆。

```java
class MedianFinder {
    Queue<Integer> A, B;
    public MedianFinder() {
        A = new PriorityQueue<>(); // 小顶堆，保存较大的一半
        B = new PriorityQueue<>((x, y) -> (y - x)); // 大顶堆，保存较小的一半
    }
    public void addNum(int num) {
        if(A.size() != B.size()) {
            A.add(num);
            B.add(A.poll());
        } else {
            B.add(num);
            A.add(B.poll());
        }
    }
    public double findMedian() {
        return A.size() != B.size() ? A.peek() : (A.peek() + B.peek()) / 2.0;
    }
}
```

