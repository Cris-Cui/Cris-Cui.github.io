# 排序算法

## 冒泡排序Bubble Sort

相邻两个元素进行大小比较 若前比后大 则互换

1. 改进一: 处理在排序过程中数组整体已经有序的情况
2. 改进二: 数组局部有序（尾部有序）

```C++
void bubble_sort(vector<int>& arr) { // 优化冒泡
    int traverse = arr.size() - 1;
    for (unsigned int i = 0; i < arr.size(); i ++) {
        int mark = 0; // 标记
        for (int j = 0; j < traverse; j ++) {
            if(arr[j] > arr[j+1]) {
                swap(arr[j], arr[j+1]);
                mark = j + 1;
            }
        } 
        if(!mark) break;
        traverse = mark;
    }
}
```

冒泡是属于交换排序类算法，它重复地走过要排序的数列，一次比较相邻两个元素，如果他俩顺序错误就把他俩交换过来（例如从小到大排列，一次比较就将大的元素排后面，小的元素排前面）一次遍历可以将序列中最大的元素浮到序列的末尾，重复地遍历N次，每一次都可以确定在无序区中最大的元素，将他浮到有序区的第一个元素。所以它的平均时间复杂度是O(n^2)，冒泡的过程只涉及相邻数据的交换操作，只需要常量级的临时空间，所以它的空间复杂度为 O(1)

冒泡是稳定的排序，当有相邻两个元素大小相等的时候，不做交换，不改变相同大小的数据的前后次序。

算法步骤：

      	1. 比较相邻元素，如果后者比前者小，就交换他们俩个。
	    	2. 对无序区每一对相邻元素做同样工作，这样会确定无序区的最大值浮到最后
	   	3. 将该最大值划为有序区，针对无序区的所有元素重复以上步骤，直到所有元素都处于有序区。

算法优化：

1. 针对排序过程中，序列整体已经有序：

	使用标记，当一次遍历过程中，没有发生任何交换，说明序列整体有序。结束遍历。

2. 针对排序过程中，无序区的末尾局部已经在物理上属于有序区（即，还没操作它，但该局部序列已经按从小到大排列且最大值比有序区小）每一次遍历无序区时，对无序区右边界的定义为上一次遍历最后一次发送交换的位置

## 快速排序Quick Sort

挖坑填补法：小的放左边，大的放右边
主要思想：分治

1.	确定分界点：q[l] q[(l+r)/2] q[r]左边界或者中间值或者右边界
2.	调整区间，使得所有小于等于x的数在x的左边，所有大于等于x的数在x的右边
3.	递归给左边排序，递归给右边排序

不稳定的来源：左右交换的时候，两个相同的数据也会交换

```C++
void QuickSort(int arr[], int l, int r) { // 快速排序
    // 找一个标准值, 将比标准值小的，都放在它的左侧，将比标准值大的，都放在它的右侧
    // 根据标准值位置，将数据分割成两部分，各部分分别重复以上操作
    if (l >= r) return;

    int i = l - 1, j = r + 1, x = arr[l + r >> 1];
    while (i < j)
    {
        do i ++ ; while (arr[i] < x);
        do j -- ; while (arr[j] > x);
        if (i < j) swap(arr[i], arr[j]);
    }
    QuickSort(arr, l, j), QuickSort(arr, j + 1, r);
}
```

## 归并排序Merge Sort

将多个有序数组进行合并

1.	确定分界点mid = l+r >> 1;
2.	递归排序，left，right
3.	归并—合二为一
	稳定

```cpp
void merge_sort(int q[], int l, int r)
{
    if (l >= r) return;

    int mid = l + r >> 1;
    merge_sort(q, l, mid);
    merge_sort(q, mid + 1, r);

    int k = 0, i = l, j = mid + 1;
    while (i <= mid && j <= r)
        if (q[i] <= q[j]) tmp[k ++ ] = q[i ++ ];
        else tmp[k ++ ] = q[j ++ ];

    while (i <= mid) tmp[k ++ ] = q[i ++ ];
    while (j <= r) tmp[k ++ ] = q[j ++ ];

    for (i = l, j = 0; i <= r; i ++, j ++ ) q[i] = tmp[j];
}
```

## 选择排序 Select Sort

遍历 每次选最大放最后 或最小放最前

```C++
void SelectSort(int arr[], int nLength) {  // 选择排序
    if (arr == NULL || nLength <= 0) return;
    for (int i = 0; i < nLength; i++) {
        int nMin = i;
        for (int j = i; j < nLength; j++) 
            if (arr[j] < arr[nMin]) 
                nMin = j;

        // 交换
        if (nMin != i) 
            swap(arr[i], arr[nMin]);
    }
}
```

## 插入排序 Insert Sort  

将待排序数据分成两部分，一部分有序一部分无序
将无序元素依次插入到有序中完成排序

Sort插入 适用于元素特别少 ≤15/元素排序前的位置距排好序的最终位置不远的时候使用插入排序

```C++
void InsertSort(int arr[], int nLength) {  // 插入排序
    // 应用场合：
    // 1. 元素特别少的时候， <= 15
    // 2. 元素排序前的位置距其排好序的最终位置不远
    if (arr == NULL || nLength <= 0) return;
    int j;
    for (int i = 1; i < nLength; i++) {
        // j 是有序的最后一个元素 || i 是无序的第一个元素
        int temp = arr[i];                 // 保存元素
        for(j = i - 1;j >= 0 && arr[j] > temp; j --)   // 倒序遍历有序序列
            arr[j + 1] = arr[j];
        arr[j + 1] = temp;
    }
}
```

## 希尔排序 Shell Sort 

将数据分组，各组内进行插入排序（缩小增量排序）。希尔排序是非稳定排序算法。

希尔排序是基于插入排序的以下两点性质而提出改进方法的：

- 插入排序在对几乎已经排好序的数据操作时，效率高，即可以达到线性排序的效率
- 但插入排序一般来说是低效的，因为插入排序每次只能将数据移动一位

```C++
void ShellSort(int arr[], int nLength) {  // 希尔排序 (缩小增量排序)
    // 希尔排序：将数据分租，各组之内插入排序 
    if (arr == NULL || nLength <= 0) return;
    // 分组间隔
    for (int nGap = nLength >> 1; nGap > 0; nGap = nGap >> 1) 
        // 有nGap组
        for (int i = 0; i < nGap; i++) 
            //各组之内插入排序
            for (int j = i + 1; j < nLength; j += nGap) {
                int k;  // k 是有序的最后一个元素 || j 是无序的第一个元素
                int temp = arr[j];                 // 保存元素
                for (k = j - nGap; k >= i && arr[k] > temp; k -= nGap)   // 倒序遍历有序序列
                    arr[k + nGap] = arr[k];  
                arr[k + nGap] = temp;
            }
}
```

## 计数排序 Counting Sort 

适用于 分配密集并且重复出现次数较多的数据

（1）找出待排序的数组中最大和最小的元素

（2）统计数组中每个值为i的元素出现的次数，存入数组C的第i项

（3）对所有的计数累加（从C中的第一个元素开始，每一项和前一项相加）

（4）反向填充目标数组：将每个元素i放在新数组的第C(i)项，每放一个元素就将C(i)减去1

```c++
void CountingSort(int arr[], int nLength) { // 计数排序 -- 基于非比较的排序
	// 分配密集，并且重复出现次数比较多的数据
    if (arr == NULL || nLength <= 0) return;
    // 最大值 最小值
    int nMin = arr[0];
    int nMax = arr[0];
    for (int i = 1;i < nLength; i ++) {
        if (arr[i] < nMin) nMin = arr[i];
        if (arr[i] > nMax) nMax = arr[i];
    }
    // 计数
    int pMark[nMax-nMin+1] = {0};

    for (int i = 0; i < nLength; i ++) {
        pMark[arr[i] - nMin] ++;
    }
    int p = 0;
    for (int i = 0; i < nMax-nMin+1; i ++) {
    	while(pMark[i]--) {
        	arr[p++] = nMin+i;
        }
    }
}
```

排名 成绩单
1、Min max
2、计数数组
3、排名计算
4、Malloc申请新的空间
5、将数据放到合适的位置 倒序遍历原数组 找到对应的排名位置 将数据放进去 排名-1
6、将新空间的数据放回原数组

## 堆排序 Heap Sort

> 大顶堆：任一一个父亲的值都比孩子都大   小顶堆：任一一个父亲的值都比孩子都小

1、建堆 
2、浮出堆顶，将堆顶元素与最后一个元素做交换，移出最后一个元素，对堆顶元素做heapify

树型结构用数组来存：  完全二叉树的性质：

>  左孩子在2i+ 1 <= n – 1  右孩子在2i + 2 <= n – 1  父结点在(i - 1) / 2   层数：log2N + 1

```C++
void heapify(int arr[], int nLength, int i) {
    // parent结点为 (i - 1) / 2
    int c1 = 2 * i + 1, c2 = 2 * i + 2; // c1结点为2*i + 1; c2结点为2*i + 2
    int max = i;
    if(c1 < nLength && arr[c1] > arr[max]) max = c1;
    if(c2 < nLength && arr[c2] > arr[max]) max = c2;
    if (max != i) swap(arr[i], arr[max]), heapify(arr, nLength, max);
}
void HeapSort(int arr[], int nLength) {
    // 1. 建大顶堆
    int last_node = nLength - 1;
    int parent = (last_node - 1) >> 1;
    for (int i = parent; i >= 0; i--) {
        heapify(arr, nLength, i);
    }
    // 2. 排序，浮出堆顶元素
    for (int i = nLength -1; i >= 0; i --) {
        swap(arr[i], arr[0]);
        heapify(arr, i, 0); //每次heapify的大小减1, 对根结点进行建堆
    }
}
```



## 桶排序 Bucket Sort

1. 找出待排序数组中的最大值max、最小值min
2. 拆位，拆出最大值的高位和最小值的高位
3. 申请组，组大小为max高位-min高位+1
4. 元素按高位入组
5. 各组之内进行排序
6. 返回原数组

> 桶排序可用于最大最小值相差较大的数据情况，但桶排序要求数据的分布必须均匀，否则可能导致数据都集中到一个桶中。

```C++
void BucketSort(int arr[], int nLength) { // 桶排序
    if (arr == NULL || nLength <= 0) return;
    // 最大值 最小值
    int nMin = arr[0];
    int nMax = arr[0];
    for (int i = 1;i < nLength; i ++) {
        if (arr[i] < nMin) nMin = arr[i];
        if (arr[i] > nMax) nMax = arr[i];
    }
    // 拆最高位
    int num = nMin;
    int count = 1;
    while(num) {
        num /= 10;
        count *= 10;
    }
    count /= 10;
    int nMinIndex = nMin / count;
    int nMaxIndex = nMax / count;
    // 申请桶空间
    Bucket **pBucket = NULL;
    pBucket = (Bucket**)malloc(sizeof(Bucket*) * (nMaxIndex - nMinIndex + 1));
    memset(pBucket, 0, sizeof(Bucket*) * (nMaxIndex - nMinIndex + 1));

    // 入桶
    Bucket *temp = NULL;
    for(int i = 0; i < nLength; i ++) {
        int index = arr[i]/count-nMinIndex;
        temp = (Bucket*)malloc(sizeof(Bucket));
        temp->nValue = arr[i];
        temp->next = arr[index];
        arr[index] = temp;
    }

    // 桶内排序
    for (int i = 0; i < nMaxIndex - nMinIndex + 1; i ++) {
        Bucket* pNode = pBucket[0];
        Bucket* ptemp = NULL;
        while(pNode->next != NULL) {
            ptemp = pNode;
            while(ptemp->next != NULL) {
                if (ptemp->nValue > ptemp->next->nValue) 
                    swap(ptemp->nValue, ptemp->next->nValue);
                ptemp = ptemp->next;
            }
        }
    }
    // 放回
    int num = 0;
    for (int i = 0; i < nMaxIndex - nMinIndex + 1; i ++) {
        temp = pBucketp[i];
        while(temp) {
            arr[num++] = temp->nValue;
            temp = temp->next;
        }
    }
    // 释放链表空间

}
```



## 基数排序 Radix Sort

> 基数排序(Radix Sort)是桶排序的扩展，它的基本思想是：将整数按位数切割成不同的数字，然后按每个位数分别比较。
> 排序过程：将所有待比较数值（正整数）统一为同样的数位长度，数位较短的数前面补零。然后，从最低位开始，依次进行一次排序。这样从最低位排序一直到最高位排序完成以后, 数列就变成一个有序序列。

1. 取得数组中的最大数，并取得位数；
2. arr为原始数组，从最低位开始取每个位组成radix数组；
3. 对radix进行计数排序（利用计数排序适用于小范围数的特点）；

```c++
void radixsort(int arr[], int nLength) {//基数排序
    int d = maxbit(arr, nLength); //取得数组中的最大数，并取得位数
    int *tmp = new int[nLength];
    int *count = new int[10]; //计数器
    int k;
    int radix = 1;
    for(int i = 1; i <= d; i++) //进行d次排序
    {
        for(int j = 0; j < 10; j++) count[j] = 0; //每次分配前清空计数器
        for(int j = 0; j < nLength; j++)
        {
            k = (arr[j] / radix) % 10; //统计每个桶中的记录数
            count[k]++;
        }
        for(int j = 1; j < 10; j++) count[j] = count[j - 1] + count[j]; //将tmp中的位置依次分配给每个桶
        for(int j = nLength - 1; j >= 0; j--) //将所有桶中记录依次收集到tmp中
        {
            k = (arr[j] / radix) % 10;
            tmp[count[k] - 1] = arr[j];
            count[k]--;
        }
        for(int j = 0; j < nLength; j++) arr[j] = tmp[j];//将临时数组的内容复制到data中
        radix *= 10;
    }
    delete []tmp;
    delete []count;
}   
```

## 时间复杂度比较

| 排序方法 | 最好时间复杂度 | 平均时间复杂度 | 最坏时间复杂度 | 空间复杂度 | 是否稳定(数值相同的两个元素在排序前后其相对位置未发生变化) |
| -------- | -------------- | -------------- | -------------- | ---------- | ---------------------------------------------------------- |
| 冒泡排序 | O(n)           | O(n^2)         | O(n^2)         | O(1)       | 稳定                                                       |
| 选择排序 | O(n)           | O(n^2)         | O(n^2)         | O(1)       | 不稳定                                                     |
| 插入排序 | O(n)           | O(n^2)         | O(n^2)         | O(1)       | 稳定                                                       |
| 希尔排序 | O(n)           | O(nlogn)       | O(ns)          | O(1)       | 不稳定                                                     |
| 计数排序 | O(n+k)         | O(n+k)         | O(n+k)         | O(n+k)     | 稳定                                                       |
| 快速排序 | O(nlogn)       | O(nlogn)       | O(n^2)         | O(nlogn)   | 不稳定                                                     |
| 归并排序 | O(nlogn)       | O(nlogn)       | O(nlogn)       | O(n)       | 稳定                                                       |
| 堆排序   | O(nlogn)       | O(nlogn)       | O(nlogn)       | O(1)       | 不稳定                                                     |
| 桶排序   | O(n)           | O(n)           | O(n)           | O(m)       | 稳定                                                       |

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