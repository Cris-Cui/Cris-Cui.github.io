# [剑指 Offer II 077. 链表排序](https://leetcode.cn/problems/7WHec2/)

## 题目
> 给定链表的头结点 head ，请将其按 升序 排列并返回 排序后的链表 。

**示例 1：**

![](https://assets.leetcode.com/uploads/2020/09/14/sort_list_1.jpg)

> 输入：head = [4,2,1,3]
输出：[1,2,3,4]


**示例 2：**
![](https://assets.leetcode.com/uploads/2020/09/14/sort_list_2.jpg)

> 输入：head = [-1,5,3,4,0]
输出：[-1,0,3,4,5]

**示例 3：**

> 输入：head = []
输出：[]
 

**提示：**

链表中节点的数目在范围 [0, 5 * 104] 内
-105 <= Node.val <= 105
 

**进阶：** 你可以在 O(n log n) 时间复杂度和常数级空间复杂度下，对链表进行排序吗？

## 解法
### 两次遍历链表, 记录+修改(暴力)
#### 代码
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        vector<int> nums;
        auto cur = head;
        while(cur != nullptr) {
            nums.push_back(cur->val);
            cur = cur->next;
        }
        sort(nums.begin(), nums.end());
        cur = head;
        for (auto i : nums) {
            cur->val = i;
            cur = cur->next;
        }
        return head;
    }
};
```
#### 复杂度分析
- 时间复杂度: O(n + nlogn) 两次遍历链表+一次快排
- 空间复杂度: O(n) vec向量记录结点的值

### 小顶堆
建小顶堆：链表所有结点入堆; 
依次弹出队头元素, 重新连接
#### 代码
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    struct Compare {
        bool operator()(ListNode* a, ListNode* b) {
            return a->val > b->val; // 使用大于符号来定义小顶堆
        }
    };
    ListNode* sortList(ListNode* head) {
        priority_queue<ListNode*, std::vector<ListNode*>, Compare> heap;
        ListNode* cur = head;
        while(cur != nullptr) {
            heap.push(cur);
            cur = cur->next;
        }
        ListNode* dummy = new ListNode(-1);
        cur = dummy;
        while(!heap.empty()) {
            cur->next = heap.top();
            heap.pop();
            cur = cur->next;
        }
        return dummy->next;
    }
};
```
#### 复杂度分析
- 时间复杂度: O(nlogn)
- 空间复杂度: O(n)