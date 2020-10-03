# 1-15

## 04. 二维数组中的查找

题设：在一个 n * m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

示例：

现有矩阵 matrix 如下：

```
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```


给定 target = 5，返回 true。

给定 target = 20，返回 false。

思路：

- 暴力

	未利用排序特点，为n^2复杂度。

- 线性查找

	可以发现以右上角的元素为假设根，整个矩阵成为一个类似于二叉查找树的结构，即可在线性复杂度内完成。

```java
class Solution {
    public boolean findNumberIn2DArray(int[][] matrix, int target) {
        if (matrix.length == 0 || matrix[0].length == 0) {
            return false;
        }
        int i = 0, j = matrix[0].length - 1, cur;
        while (i < matrix.length && j >= 0) {
            cur = matrix[i][j];
            if (cur == target) {
                return true;
            }
            if (cur > target) {
                j--;
            } else {
                i++;
            }
        }
        return false;
    }
}
```

## 06. 从尾到头打印链表

题设：略

思路：

- mine：按顺序遍历原链表，同时不断使当前结点作为临时头结点的下一个结点。
- **递归法**：递归打印之后所有结点，再打印出当前结点。临时栈：和自己方法类似，自己实际上做完了翻转链表的过程。

```java
 public int[] reversePrint(ListNode head) {
        ListNode tmp = new ListNode(-1), cur = head;
        int len = 0, cnt = 0;
        while (cur != null) {
            head = head.next;
            cur.next = tmp.next;
            tmp.next = cur;
            cur = head;
            len++;
        }
        tmp = tmp.next;
        int[] res = new int[len];
        while (tmp != null) {
            res[cnt++] = tmp.val;
            tmp = tmp.next;
        }
        return res;
    }
```

## 07. 重建二叉树

题设：输入某二叉树的前序遍历和中序遍历的结果，请重建该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。

例如，给出

前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
返回如下的二叉树：

        3
       / \
      9  20
        /  \
       15   7
思路：

- 抓住前序遍历和中序遍历的特点：前序遍历序列构成->(根，左子树，右子树)。中序遍历序列构成->(左子树，根，右子树)。

```java
public TreeNode build(int[] preorder, int[] inorder, int prestart, int preend, int instart, int inend) {
        if (prestart > preend || instart > inend) {
            return null;
        }
        TreeNode root = new TreeNode(preorder[prestart]);
        int i;
        for (i = instart; i <= inend; i++) {
            if (inorder[i] == preorder[prestart]) {
                break;
            }
        }
        int leftlength = i - instart;
        root.left = build(preorder, inorder, prestart + 1, prestart + leftlength, instart, i - 1);
        root.right = build(preorder, inorder, prestart + leftlength + 1, preend, i + 1, inend);
        return root;
    }
```

## 09. 用两个栈实现队列

题设：用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )

思路：一个入队栈，一个出队栈。入队时：直接入入队栈；出队时：若出队栈不为空，则弹出栈顶元素，若为空，将入队栈依次入出队栈，再从出队栈弹出。

```java
class CQueue {

    private LinkedList<Integer> stacka;
    private LinkedList<Integer> stackb;

    public CQueue() {
        this.stacka = new LinkedList<>();
        this.stackb = new LinkedList<>();
    }

    public void appendTail(int value) {
        this.stackb.add(value);
    }

    public int deleteHead() {
        if (stacka.isEmpty()) {
            while (!stackb.isEmpty()) {
                stacka.add(stackb.removeLast());
            }
        }
        if (stacka.isEmpty()) {
            return -1;
        } else {
            return stacka.removeLast();
        }
    }
}
```

相似：两个队列实现一个栈，思想类似。

## 10- I. 斐波那契数列 10- II. 青蛙跳台阶问题

思想类似，以第二题为例。

题设：一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个 n 级的台阶总共有多少种跳法。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

思路：设f(n)为n级台阶的最优解，很容易得到状态转移方程：f(n) = f(n - 1) + f(n - 2)。

```java
//动态规划直接用对应维数数组解决更佳。
class Solution {
    static int[] vis = new int[101];
    public int numWays(int n) {
        if (n == 0 || n == 1) {
            return vis[n] = 1;
        }
        if (vis[n - 2] == 0) {
            vis[n - 2] = numWays(n - 2);
        }
        if (vis[n - 1] == 0) {
            vis[n - 1] = numWays(n - 1);
        }
        vis[n] = (vis[n - 2] + vis[n - 1]) % 1000000007;
        return vis[n];
    }
}
```

## 11. 旋转数组的最小数字

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组 [3,4,5,1,2] 为 [1,2,3,4,5] 的一个旋转，该数组的最小值为1。  

示例 1：

输入：[3,4,5,1,2]
输出：1

示例 2：

输入：[2,2,2,0,1]
输出：0

思路：常规遍历的复杂度为线性。选起始元素作为标志不能区分在左右哪个区间，所有选右侧元素。

```java
class Solution {
    public int minArray(int[] numbers) {
        int len = numbers.length;
        int start = 0, end = len - 1, mid = 0;
        while (start < end) {
            mid = (start + end) / 2;
            if (numbers[mid] > numbers[end]) {
                start = mid + 1;
            } else if (numbers[mid] < numbers[end]) {
                end = mid;
            } else {
                end = end - 1;
            }
        }
        return numbers[start];
    }
}
```

## 12. 矩阵中的路径*

题设：请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一格开始，每一步可以在矩阵中向左、右、上、下移动一格。如果一条路径经过了矩阵的某一格，那么该路径不能再次进入该格子。例如，在下面的3×4的矩阵中包含一条字符串“bfce”的路径（路径中的字母用加粗标出）。

[["a","b","c","e"],
["s","f","c","s"],
["a","d","e","e"]]

但矩阵中不包含字符串“abfb”的路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入这个格子。

思路：dfs搜索，当搜索次数超过字符串长度，或搜索到的字符和对应位置字符不同时返回，一个起点是否可行为向其它四条路径探索所有结果的或。

```java
class Solution {
    static boolean[][] vis;

    public static boolean find(char[][] board,int i, int j, String word, int time) {
        if (!vis[i][j] && board[i][j] == word.charAt(time)) {
            vis[i][j] = true;
            if (time + 1 == word.length()) {
                return true;
            } else {
                boolean res = false;
                if (j + 1 < board[0].length) {
                    res = find(board, i, j + 1, word, time + 1);
                }
                if (j > 0) {
                    res = (res || find(board, i, j - 1, word, time + 1));
                }
                if (i + 1 < board.length) {
                    res = (res || find(board, i + 1, j, word, time + 1));
                }
                if (i > 0) {
                    res = (res || find(board, i - 1, j, word, time + 1));
                }
                 if (!res) {
                    vis[i][j] = false;
                }
                return res;
            }
        }
        return false;
    }

    public boolean exist(char[][] board, String word) {
        if ("".equals(word)) {
                return false;
        }
        int row = board.length, col = board[0].length, len = word.length();
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                vis = new boolean[201][201];
                if (find(board, i, j, word, 0)) {
                    return true;
                }
            }
        }
        return false;
    }
}
```

## 13. 机器人的运动范围

简单的dfs。

## 14- I. 剪绳子

题设：给你一根长度为 n 的绳子，请把绳子剪成整数长度的 m 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 k[0],k[1]...k[m-1] 。请问 k[0]*k[1]*...*k[m-1] 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

思路：对于一段绳子，分成两段后，最佳切割为两段切或不切的四种组合的最大值。

```java
class Solution {
    static int[] arr = new int[58];
    public int cuttingRope(int n) {
       arr[1] = 1;
        for (int i = 2; i <= n; i++) {
            int res = 0;
            for (int j = 1; j <= i / 2; j++) {
                int a = Math.max(arr[j], j);
                int b = Math.max(arr[i - j], i - j);
                res = Math.max(res, a * b);
            }
            arr[i] = res;
        }
        return arr[n];
    }
}
```

## 14- II. 剪绳子 II

题设：同上

此时数据加大，dp数组无法储存中间结果，只能用贪心(-3-2)。

3*n次方使用快速幂，每个中间结果都进行取模。

```java
//快速幂
public int pow(int base, int n) {
  int res = 1;
  while (n > 0) {
    if (n & 1 == 1) {
      res *= base;
    }
    base *= base;
    n >>>= 1;
  }
  return res;
}
```

## 15. 二进制中1的个数

难度低。

知识点：n & n - 1 消去n最右侧的1，可判断是否是2的幂。