# LeetCode

## 872. Leaf-Similar Trees

Consider all the leaves of a binary tree.  From left to right order, the values of those leaves form a leaf value sequence.


For example, in the given tree above, the leaf value sequence is (6, 7, 4, 9, 8).

Two binary trees are considered leaf-similar if their leaf value sequence is the same.

Return true if and only if the two given trees with head nodes root1 and root2 are leaf-similar.

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
    public boolean leafSimilar(TreeNode root1, TreeNode root2) {
        Stack<TreeNode> s1 = new Stack<>();
        Stack<TreeNode> s2 = new Stack<>();
        
        s1.push(root1);
        s2.push(root2);
        while(!s1.empty() && !s2.empty()) {
            if(dfs(s1) != dfs(s2)){
                return false;
            }
        }
        return s1.empty()&&s2.empty();
    }
    
    public int dfs(Stack<TreeNode> s) {
        
        while(true) {
            TreeNode node = s.pop();
            if (node.right != null) {
                s.push(node.right);
            }
            if (node.left != null) {
                s.push(node.left);
            }
            if(node.left == null && node.right == null) {
                return node.val;
            }
        }
    }
}
```

* Time Complexity：O(n)
* Space Complexity: O(1)