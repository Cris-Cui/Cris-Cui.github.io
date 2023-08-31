## [剑指 Offer II 023. 两个链表的第一个重合节点](https://leetcode.cn/problems/3u1WK4/)

### 题目

给定两个单链表的头节点 `headA` 和 `headB` ，请找出并返回两个单链表相交的起始节点。如果两个链表没有交点，返回 `null` 。

图示两个链表在节点 `c1` 开始相交**：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)

题目数据 **保证** 整个链式结构中不存在环。

**注意**，函数返回结果后，链表必须 **保持其原始结构** 。

**示例 1：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_1.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_1.png)

```
输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
输出：Intersected at '8'
解释：相交节点的值为 8 （注意，如果两个链表相交则不能为 0）。
从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。
在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
```

**示例 2：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_2.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_2.png)

```
输入：intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
输出：Intersected at '2'
解释：相交节点的值为 2 （注意，如果两个链表相交则不能为 0）。
从各自的表头开始算起，链表 A 为 [0,9,1,2,4]，链表 B 为 [3,2,4]。
在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。
```

**示例 3：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_3.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_3.png)

```
输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
输出：null
解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。
由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
这两个链表不相交，因此返回 null 。
```

 

**提示：**

- `listA` 中节点数目为 `m`
- `listB` 中节点数目为 `n`
- `0 <= m, n <= 3 * 104`
- `1 <= Node.val <= 105`
- `0 <= skipA <= m`
- `0 <= skipB <= n`
- 如果 `listA` 和 `listB` 没有交点，`intersectVal` 为 `0`
- 如果 `listA` 和 `listB` 有交点，`intersectVal == listA[skipA + 1] == listB[skipB + 1]`

 

**进阶：**能否设计一个时间复杂度 `O(n)` 、仅用 `O(1)` 内存的解决方案？

### 解法

#### 暴力（计算长度，尾对齐同时遍历）

1. 分别计算两条链表的长度
2. 长度较长的一条链表先遍历到与另一条链表头齐平的位置
3. 同时遍历两条链表
	- 如果结点相同，代表相交
	- 如果遍历到链表尾，表示链表不相交

##### 代码

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    int getListLength(ListNode* root) {
        int length = 0;
        while(root != nullptr) {
            length ++;
            root = root->next;
        }
        return length;
    }
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        int length_a = getListLength(headA);
        int length_b = getListLength(headB);
        int gap = abs(length_a - length_b);
        if(length_a > length_b) {
            while(gap --) headA = headA->next;
        } else {
            while(gap --) headB = headB->next;
        }
        while(headA != nullptr && headA != headB) {
            headA = headA->next;
            headB = headB->next;
        }
        if (headA == nullptr) return nullptr;
        else return headA;
    }
};
```



##### 复杂度分析

- 时间复杂度 O(n +m), 三次遍历链表
- 空间复杂度 O(1)

#### 双指针

1. 双指针分别指向链表A和链表B的头
2. 同时移动双指针
3. 当指针为空时，修改指针指向另一条链表的头节点
	- 如果双指针指向同一个节点，就是两条链表的交点
	- 如果双指针同时指向nullptr，说明两条链表不相交

- 证明：
	设链表A的长度为n，链表B的长度为m。

	链表A和链表B相同的节点数量为x。

	- 第一种情况，链表相交（x > 0)

		指针a第二次走到第一个相交的节点的长度为: n + (m - x  + 1)

		指针b第二次走到第一个相交的节点的长度为: m + (n - x + 1)

		即第二次都走到m + n - x + 1的位置时，链表相交。

	- 第二种情况, 链表不相交( x = 0) 

		指针a 走完A和B两条链表( n + m ) 后会指向nullptr

		指针b 走完A和B两条链表( m + n ) 后也会指向nullptr

	说明，当指针a 和 b相同时，为nullpt即为链表不相交；不为nullptr即为链表相交，且为第一个相交的节点。

##### 代码

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        if (headA == nullptr || headB == nullptr) {
            return nullptr;
        }
        ListNode* curA = headA, *curB = headB;
        while(curA != curB) {
            if (curA == nullptr) curA = headB;
            else curA = curA->next;
            if (curB == nullptr) curB = headA;
            else curB = curB->next;
        }
        return curA;
    }
};
```



##### 复杂度分析

- 时间复杂度 O(n + m )
- 空间复杂度 O(1)

