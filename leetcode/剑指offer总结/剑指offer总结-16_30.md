# 16-30

## 16. 数值的整数次方

题设：实现函数double Power(double base, int exponent)，求base的exponent次方。不得使用库函数，同时不需要考虑大数问题。

直接快速幂，过程见15。

## 17. 打印从1到最大的n位数

题设：输入数字 `n`，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则打印出 1、2、3 一直到最大的 3 位数 999。

思路：直接打印能过是出题的失误，必须用字符串模拟。

- 方法一：拼接模拟，先打出1~9，然后第二轮用0-9右接前面所有一位的得到所有两位，再用0-9右接所有两位得到所有三位，最后加入一个单独0。
- 方法二：递归全排列。

## 18. 删除链表的节点

思路：建立一个临时结点和紧随其后的一个当前结点，同时移动，判断当前结点是否需要删除。

## 19. 正则表达式匹配*



## 21. 调整数组顺序使奇数位于偶数前面

题设：输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。

思路：头尾双指针，每遇到需要交换的一组数就执行交换，直到指针重合。

```java
class Solution {

    public static boolean odd(int a) {
        return ((a & 1) == 1);
    }

    public static boolean even(int a) {
        return ((a & 1) == 0);
    }

    public int[] exchange(int[] nums) {
        int i = 0, j = nums.length - 1;
        while (i < j) {
            while (odd(nums[i]) && i < j) {
                i++;
            }
            while (even(nums[j]) && i < j) {
                j--;
            }
            if (i < j) {
                int tmp = nums[i];
                nums[i] = nums[j];
                nums[j] = tmp;
            } else {
                break;
            }
        }
        return nums;
    }
}
```

## 24. 反转链表

思路：创建一个临时结点，然后在原链表头上设置双指针先后移动。

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode tmp = new ListNode(-1000000);
        ListNode pointer;
        while (head != null) {
            pointer = head;
            head = head.next;
            pointer.next = tmp.next;
            tmp.next = pointer;
        }
        return tmp.next;
    }
}
```

## 26. 树的子结构

题设：输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)

B是A的子结构， 即A中有出现和B相同的结构和节点值。

例如:
给定的树 A:

​    3
   / \  

 4   5
  / \
 1   2
给定的树 B：

   4 
  /
 1

返回true，因为B与A的一个子树拥有相同的结构和节点值。

思路：如果根结点相同，如果存在左右结点的话也必须相同，并递归左右子树；如果根结点不相同，则直接递归左右子树。

```java
class Solution {
    public boolean isSubStructure(TreeNode A, TreeNode B) {
         if (B == null || A == null) {
            return false;
        }
        boolean res1 = (A.val == B.val);
        if (B.left != null) {
            res1 = (res1 && isSubStructure(A.left, B.left) && A.left.val == B.left.val);
        }
        if (B.right != null) {
            res1 = (res1 && isSubStructure(A.right, B.right) && A.right.val == B.right.val);
        }
        boolean res2 = (isSubStructure(A.right, B) || isSubStructure(A.left, B));
        return (res1 || res2);
    }
}
```

## 27. 二叉树的镜像

思路：交换根节点的左右结点后，对交换后的左右子树进行递归。（先序和后序两种方式）。

```java
//先序
class Solution {

    public void adjust(TreeNode root) {
        TreeNode tmp = root.left;
        root.left = root.right;
        root.right = tmp;
        if (root.left != null) {
            adjust(root.left);
        }
        if (root.right != null) {
            adjust(root.right);
        }
    }

    public TreeNode mirrorTree(TreeNode root) {
        if (root == null) {
            return null;
        }
        adjust(root);
        return root;
    }
}
```

## 28. 对称的二叉树

思路：判断二叉树对称，即要判断对称处的结点是否相等。给定一颗树，其根结点不影响对称性，第一组对称发生在左右子结点上，即判断完左右子结点后，递归判断左左和右右结点，左右右左结点。

```java
class Solution {

    public static boolean judge(TreeNode root1, TreeNode root2) {
        if (root1 == null && root2 == null) {
            return true;
        }
        if (root1 != null && root2 != null) {
            return (root1.val == root2.val) && judge(root1.left, root2.right) && judge(root1.right, root2.left);
        }
        return false;
    }

    public boolean isSymmetric(TreeNode root) {
        if (root == null) {
            return true;
        }
        return judge(root.left, root.right);
    }
}
```

