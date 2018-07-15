# 1. LeetCode

## 230. Kth Smallest Element in a BST

Given a binary search tree, write a function kthSmallest to find the kth smallest element in it.

Note: 
You may assume k is always valid, 1 ≤ k ≤ BST's total elements.

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        int left = size(root.left);
        if (left == k - 1) {
            return root.val;
        }
        if (left >= k) {
            return kthSmallest(root.left, k);
        }else {
            return kthSmallest(root.right, k - left - 1);            
        }
    }
    
    private int size(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int left = size(root.left);
        int right = size(root.right);
        return left + right + 1;
    }
}

/*
本题的目标是找到BST中的第k小。
BST树结构的特点：以根结点为界限，左边小，右边大。
private int size(TreeNode root) 该函数用来获取root为根结点的树的大小，即树的节点的个数。
主函数流程：
1. 首先获取左子树的大小，若左子树的大小为 k - 1， 则根结点为要找的节点，返回根结点的值。
2. 若 左子树的大小大于k，则在左子树中继续寻找第k小。
3. 若左子树的大小小于k，则在右子树中寻找第 k - left - 1 小。
*/
```



* Time Complexity：
* Space Complexity: 



##2. Review

https://github.com/huatangzhi/ARTS/blob/master/ARTS/%20002/Review.md

## 3. Tips

https://github.com/huatangzhi/ARTS/blob/master/ARTS/%20002/Tip.md

## 4. Shares

https://github.com/huatangzhi/ARTS/blob/master/ARTS/%20002/Share.md


