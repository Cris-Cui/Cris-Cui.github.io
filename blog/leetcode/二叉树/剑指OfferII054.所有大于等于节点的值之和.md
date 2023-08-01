### [剑指 Offer II 054. 所有大于等于节点的值之和](https://leetcode.cn/problems/w6cpku/)

#### 题目

> 给定一个二叉搜索树，请将它的每个节点的值替换成树中大于或者等于该节点值的所有节点值之和。
>
> 提醒一下，二叉搜索树满足下列约束条件：

*   节点的左子树仅包含键\*\* 小于 \*\*节点键的节点。
*   节点的右子树仅包含键\*\* 大于\*\* 节点键的节点。
*   左右子树也必须是二叉搜索树。

 

**示例 1：**

**![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/05/03/tree.png)**

```
输入：root = [4,1,6,0,2,5,7,null,null,null,3,null,null,null,8]
输出：[30,36,21,36,35,26,15,null,null,null,33,null,null,null,8]

```

**示例 2：**

```
输入：root = [0,null,1]
输出：[1,null,1]

```

**示例 3：**

```
输入：root = [1,0,2]
输出：[3,3,2]

```

**示例 4：**

```
输入：root = [3,2,4,1]
输出：[7,9,4,10]

```

 

**提示：**

*   树中的节点数介于 `0` 和 `10`^4^^ ^之间。
*   每个节点的值介于 `-10`^4^ 和 `10`^4^ 之间。
*   树中的所有值 **互不相同** 。
*   给定的树为二叉搜索树。

#### 解法

##### 中序遍历+前缀和

*   由于这是一棵二叉搜索树，那么利用BST的性质可知中序遍历结果是有序的，也就能求出中序数组的前缀和
*   再中序遍历一次二叉树，修改每个结点的val == 该二叉树的结点之和 - 该结点前一个结点的前缀和

###### 代码

```cpp

class Solution {
public:
    vector<int> nums;
    void inorder(TreeNode* cur) {
        if (cur == nullptr) return;
        inorder(cur->left);
        nums.push_back(cur->val);
        inorder(cur->right);
    }

    void prefixsum() {
        int sum = 0;
        for (int i = 0; i < nums.size(); i ++) {
            sum += nums[i];
            nums[i] = sum;
        }
    }
    int i = 0;
    bool flag = true;
    void in(TreeNode* cur) {
        if (cur == nullptr) return;
        in(cur->left);
        if (flag) {
            cur->val = nums[nums.size() - 1];
            flag = false;
        }
        else cur->val = nums[nums.size() - 1] - nums[i++];
        in(cur->right);
    }

    TreeNode* convertBST(TreeNode* root) {
        // 中序算前缀和
        inorder(root);
        prefixsum();
        in(root);
        return root;
    }
};
```

###### &#x20;复杂度分析

*   时间复杂度: O(n) 两次中序遍历和一次遍历中序数组
*   空间复杂度: O(n) nums数组和递归调用栈

##### 反序中序遍历(右根左)

反序中序遍历该二叉搜索树，记录过程中的节点值之和，并不断更新当前遍历到的节点的节点值

###### 代码

```cpp
class Solution {
public:
    int sum = 0;
    void inorder(TreeNode* root) {
        if (root == nullptr) return;
        inorder(root->right);
        sum += root->val;
        root->val = sum;
        inorder(root->left);
    }
    TreeNode* convertBST(TreeNode* root) {
        inorder(root);
        return root;
    }
};
```

###### 复杂度分析

*   时间复杂度: O(n) 中序遍历一次
*   空间复杂度: O(n) 递归调用栈

