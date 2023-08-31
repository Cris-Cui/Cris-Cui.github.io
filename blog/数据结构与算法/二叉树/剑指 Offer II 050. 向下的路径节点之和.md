## [剑指 Offer II 050. 向下的路径节点之和](https://leetcode.cn/problems/6eUYwP/)

### 题目

给定一个二叉树的根节点 `root` ，和一个整数 `targetSum` ，求该二叉树里节点值之和等于 `targetSum` 的 **路径** 的数目。

**路径** 不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/04/09/pathsum3-1-tree.jpg)

```
输入：root = [10,5,-3,3,2,null,11,3,-2,null,1], targetSum = 8
输出：3
解释：和等于 8 的路径有 3 条，如图所示。
```

**示例 2：**

```
输入：root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
输出：3
```

**提示:**

- 二叉树的节点个数的范围是 `[0,1000]`
- `-109 <= Node.val <= 109` 
- `-1000 <= targetSum <= 1000` 

### 解法

#### 前缀和+回溯

1. 利用前缀和的思想，可以求得目标`target`的路径和——》前缀之差

- 前缀数组`preSum`存放每一个前缀和-目标值的差存在的个数

2. 利用DFS递归处理左右子树

3. 递归处理完子树之后需要恢复现场。

##### 代码

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    unordered_map<long long,int>preSum;
    int dfs(TreeNode* node,long long cur,int targetSum){
        if(!node) return 0;
        int ret = 0;

        cur += node->val;
        if(preSum.count(cur - targetSum)){
            ret += preSum[cur - targetSum];
        }
        preSum[cur]++;
        ret += dfs(node->left,cur,targetSum);
        ret += dfs(node->right,cur,targetSum);
        preSum[cur]--;
        return ret;
    }
    int pathSum(TreeNode* root, int targetSum) {
        preSum[0] = 1;
        return dfs(root,0,targetSum);
    }
};
```

##### 复杂度分析:

- 时间复杂度O(N)
- 空间复杂度O(N)



