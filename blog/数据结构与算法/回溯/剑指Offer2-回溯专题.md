# 回溯算法

**本质: 枚举,** 穷举所有可能的结果，从中找到符合条件的答案.

**思路:** 问题抽象为一棵高度有限的N叉树

*   N叉树的高度是递归的深度
*   N叉树的宽度是集合的大小

**模板:**

```cpp
void backtrace(参数) {
    if (终止条件) {
        存放结果;
        return;
    }
    for (选择: 本层集合中的元素) {
        处理结点;
        backtrace(路径, 选择列表);
        回溯;
    }
}
```

<details>
## 子集问题

> 找N叉树的所有结点

### [剑指 Offer II 079. 所有子集](https://leetcode.cn/problems/TVdhkn/)

#### 题目

> 给定一个整数数组 nums ，数组中的元素 互不相同 。返回该数组所有可能的子集（幂集）。
> 解集 不能 包含重复的子集。你可以按 任意顺序 返回解集。

**示例 1：**

```
输入：nums = [1,2,3]
输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]

```

**示例 2：**

```
输入：nums = [0]
输出：[[],[0]]

```

**提示：**

*   `1 <= nums.length <= 10`
*   `-10 <= nums[i] <= 10`
*   `nums` 中的所有元素 **互不相同**

#### 解法

##### 回溯

![image.png](https://note.youdao.com/yws/res/10168/WEBRESOURCE496177e9ecd3d718021e6f6f44d75681)

回溯三部曲解答:&#x20;

1.  递归函数的返回值以及参数
    *   定义全局结果数组result和路径数组path, 所以不需要返回值, 参数传入原数组和每层递归从哪个index开始横向遍历

2.  回溯函数终止条件

```cpp
index >= nums.size()
```

子集是组合而不是排列, 无序, {1, 2}和{2, 1} 其实是一个子集

1.  单层搜索的过程
    将当前结点加入到结果数组中, 递归寻找剩余结点, 在每层递归开始前将路径path数组加入到结果数组中。

###### 代码

```cpp
class Solution {
public:
    vector<vector<int> > result;
    vector<int> path;
    void backtrace(vector<int>& nums, int index) {
        result.push_back(path);    // 收集每个结点
        if (index >= nums.size()) {
            return;
        }
        for (int i = index; i < nums.size(); i ++) {
            path.push_back(nums[i]);
            backtrace(nums, i + 1);
            path.pop_back();
        }
    }
    vector<vector<int>> subsets(vector<int>& nums) {      
        result.clear();
        path.clear();   
        backtrace(nums, 0);
        return result;
    }
};
```

###### 复杂度分析

*   时间复杂度: O(n×2^n^) 一共2^n^个状态，每种状态需要O(n)的时间来构造子集。
*   空间复杂度: O(n)

</details>
## 组合问题

> 收集树的叶子节点

<details>
### [剑指 Offer II 080. 含有 k 个元素的组合](https://leetcode.cn/problems/uUsW3B/)

#### 题目

> 给定两个整数 `n` 和 `k`，返回 `1 ... n` 中所有可能的 `k` 个数的组合。

**示例 1:**

    输入: n = 4, k = 2
    输出:
    [
      [2,4],
      [3,4],
      [2,3],
      [1,2],
      [1,3],
      [1,4],
    ]

**示例 2:**

    输入: n = 1, k = 1
    输出: [[1]]

**提示:**

*   `1 <= n <= 20`
*   `1 <= k <= n`

#### 解法

##### 回溯

以例1为例:
![image.png](https://note.youdao.com/yws/res/10231/WEBRESOURCE7d3258ea84a07d4f2e7ca174340b9f94)

回溯三部曲解答:

1.  递归函数的返回值以及参数
    *   定义全局结果数组result和路径数组path, 所以不需要返回值, 参数每次横向遍历的起始index和当前递归深度

2.  回溯函数终止条件

```cpp
depth == k
```

子集是组合而不是排列, 无序, {1, 2}和{2, 1} 其实是一个子集

1.  单层搜索的过程
    将当前结点加入到结果数组中, 递归寻找剩余结点,

###### 代码

```cpp
class Solution {
public:
    vector<vector<int>> result;
    vector<int> path;
    void backtrace(int n, int k, int index, int depth) {
        if (depth == k) {
            result.emplace_back(path);
            return;
        }
        for (int i = index; i <= n; i ++) {
            path.push_back(i);
            backtrace(n, k, i + 1, depth + 1);
            path.pop_back();
        }
    }
    vector<vector<int>> combine(int n, int k) {
        backtrace(n, k, 1, 0);
        return result;
    }
};
```

###### 剪枝优化

横向遍历时, `i` 应该`<= n - (k - path.size()) + 1`

</details>

### [剑指 Offer II 081. 允许重复选择元素的组合](https://leetcode.cn/problems/Ygoe9J/)

#### 题目

> 给定一个**无重复元素**的正整数数组 `candidates` 和一个正整数 `target` ，找出 `candidates` 中所有可以使数字和为目标数 `target` 的唯一组合。
>
> `candidates` 中的数字可以无限制重复被选取。如果至少一个所选数字数量不同，则两种组合是不同的。 
>
> 对于给定的输入，保证和为 `target` 的唯一组合数少于 `150` 个。

 

**示例 1：**

```
输入: candidates = [2,3,6,7], target = 7
输出: [[7],[2,2,3]]

```

**示例 2：**

    输入: candidates = [2,3,5], target = 8
    输出: [[2,2,2,2],[2,3,3],[3,5]]

**示例 3：**

```
输入: candidates = [2], target = 1
输出: []

```

**示例 4：**

```
输入: candidates = [1], target = 1
输出: [[1]]

```

**示例 5：**

```
输入: candidates = [1], target = 2
输出: [[1,1]]

```

 

**提示：**

*   `1 <= candidates.length <= 30`
*   `1 <= candidates[i] <= 200`
*   `candidate` 中的每个元素都是独一无二的。
*   `1 <= target <= 500`

#### 解法

##### 回溯

![image.png](https://note.youdao.com/yws/res/10403/WEBRESOURCE8316e6f8a434475ccd0d479d34de4a76)

回溯三部曲解答:

1.  递归函数的返回值以及参数
    *   定义全局结果数组`result`和路径数组`path`,  **纵向遍历递归没有层数的限制, 只要选取的`path`路径总和超过`target`就返回**

2.  回溯函数终止条件

    ```cpp
    sum >= target
    ```

3.  单层搜索的过程
    将当前结点加入到结果数组中, 递归寻找剩余结点

###### 代码

```cpp
class Solution {
public:
    vector<vector<int>> result;
    vector<int> path;
    void backtrace(vector<int>& nums, int index, int sum, int target) {
        if (sum > target) {
            return;
        }
        if (sum == target) {
            result.emplace_back(path);
            return;
        }
        for (int i = index; i < nums.size() && sum + nums[i] <= target; i ++) {
            path.push_back(nums[i]);
            backtrace(nums, i, sum + nums[i], target);
            path.pop_back();
        }
    }
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());
        backtrace(candidates, 0, 0, target);
        return result;
    }
};
```

###### 剪枝优化

当结果组合>= `target` ， 横向遍历不再继续

```cpp
 for (int i = index; i < nums.size() && sum + nums[i] <= target; i ++) 
```

### [剑指 Offer II 082. 含有重复元素集合的组合](https://leetcode.cn/problems/4sjJUc/)

#### 题目

> 给定一个可能有重复数字的整数数组 `candidates` 和一个目标数 `target` ，找出 `candidates` 中所有可以使数字和为 `target` 的组合。
>
> `candidates` 中的每个数字在每个组合中只能使用一次，解集不能包含重复的组合。 

**示例 1:**

    输入: candidates = [10,1,2,7,6,1,5], target = 8,
    输出:
    [
    [1,1,6],
    [1,2,5],
    [1,7],
    [2,6]
    ]

**示例 2:**

    输入: candidates = [2,5,2,1,2], target = 5,
    输出:
    [
    [1,2,2],
    [5]
    ]

**提示:**

*   `1 <= candidates.length <= 100`
*   `1 <= candidates[i] <= 50`
*   `1 <= target <= 30`

#### 解法

##### 回溯

 ![image.png](https://note.youdao.com/yws/res/10440/WEBRESOURCE77ff458298a8389bd593dae8323bd8c6)

这道题在上道题的基础上修改了题意:

*   **数组内的元素不可重复使用**

    在回溯时, 下一次的`index`应该是`i + 1`了
*   **解集不能包含重复的组合**

    同层高度（横向遍历)时去重

###### 代码

```cpp
class Solution {
public:
    vector<vector<int>> result;
    vector<int> path;
    void backtrace(vector<int>& nums, int index, int sum, int target) {
        if (sum > target) {
            return;
        }
        if (sum == target) {
            result.emplace_back(path);
            return;
        }
        for (int i = index; i < nums.size() && sum + nums[i] <= target; i ++) {
            path.push_back(nums[i]);
            backtrace(nums, i, sum + nums[i], target);
            path.pop_back();
        }
    }
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());
        backtrace(candidates, 0, 0, target);
        return result;
    }
};
```

###### 剪枝优化

当结果组合>= `target` ， 横向遍历不再继续

```cpp
 for (int i = index; i < nums.size() && sum + nums[i] <= target; i ++) 
```

## 排列问题

> 收集树的叶子节点，但与组合不同, 排列内元素有序, 所以每次下一层横向遍历时不能从index + 1开始

### [剑指 Offer II 083. 没有重复元素集合的全排列](https://leetcode.cn/problems/VvJkup/)

> 给定一个不含重复数字的整数数组 `nums` ，返回其 **所有可能的全排列** 。可以 **按任意顺序** 返回答案。

**示例 1：**

```
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]

```

**示例 2：**

```
输入：nums = [0,1]
输出：[[0,1],[1,0]]

```

**示例 3：**

```
输入：nums = [1]
输出：[[1]]

```

 

**提示：**

*   `1 <= nums.length <= 6`
*   `-10 <= nums[i] <= 10`
*   `nums` 中的所有整数 **互不相同**

#### 解法

##### 回溯

![image.png](https://note.youdao.com/yws/res/10474/WEBRESOURCE34a086d20c8bc25d9149d85fc2743c92)

回溯三部曲解答:

1.  递归函数的返回值以及参数
    *   由于排列问题是有序的, 所以不需要index + 1来控制下一层递归的横向遍历，所以需要used数组记录每个元素是否被使用

2.  回溯函数终止条件

    ```cpp
    path.size() == nums.size()
    ```

3.  单层搜索的过程&#x20;

    *   如果当前元素被使用过, continue

    *   将当前结点加入到结果数组中, 递归寻找剩余结点

    *   恢复现场

###### 代码

```cpp
class Solution {
public:
    vector<vector<int>> result;
    vector<int> path;
    void backtrace(vector<int>& nums, vector<bool>& used) {
        if (path.size() == nums.size()) {
            result.emplace_back(path);
            return;
        }
        for (int i = 0; i < nums.size(); i ++) {
            if (used[i]) continue;
            path.push_back(nums[i]);
            used[i] = true;
            backtrace(nums, used);
            path.pop_back();
            used[i] = false;
        }
    }
    vector<vector<int>> permute(vector<int>& nums) {
        vector<bool> used(nums.size(), false);
        backtrace(nums, used);
        return result;
    }
};
```

### [剑指 Offer II 084. 含有重复元素集合的全排列](https://leetcode.cn/problems/7p8L0Z/)

#### 题目

> 给定一个可包含重复数字的整数集合 `nums` ，**按任意顺序** 返回它所有不重复的全排列。

**示例 1：**

```
输入：nums = [1,1,2]
输出：
[[1,1,2],
 [1,2,1],
 [2,1,1]]

```

**示例 2：**

```
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]

```

 

**提示：**

*   `1 <= nums.length <= 8`
*   `-10 <= nums[i] <= 10`

#### 解法

##### 回溯

![image.png](https://note.youdao.com/yws/res/10494/WEBRESOURCE44d72fb67d0bb1df8096be76bb11453b "image.png")

这道题跟上道题的区别在于:

*   数组中存在重复的元素

    需要在树层(横向遍历)的时候去重

###### 代码

```cpp
class Solution {
public:
    vector<vector<int>> result;
    vector<int> path;
    void backtarce(vector<int>& nums, vector<bool>& used) {
        if (path.size() == nums.size()) {
            result.emplace_back(path);
            return;
        }
        for (int i = 0; i < nums.size(); i ++) {
            if (i > 0 && nums[i] == nums[i - 1] && used[i - 1]  == false) continue;
            if (used[i]) continue;
            used[i] = true;
            path.push_back(nums[i]);
            backtarce(nums, used);
            path.pop_back();
            used[i] = false;
        }
    }
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        vector<bool> used(nums.size(), false);
        sort(nums.begin(), nums.end());
        backtarce(nums, used);
        return result;
    }
};
```

### 细说一下去重

> 去重, **使用过**的元素不能再使用

何为使用过?

**对于一颗N叉树, 使用过有两种维度:**

*   在同一个树枝上使用过
*   在同一个树层上使用过

对于两个符合要求的path， 不重复要求的是**去重同一树层上的“使用过”**

**而同一树枝上指的是一个组合里的元素**

回到刚才的排列问题，为什么去重判定条件是`if (i > 0 && nums[i] == nums[i - 1] && used[i - 1]  == false)`

*   `used[i - 1] == false` : 说明同一树枝没有使用过`nums[i - 1]`，但现在遍历到了`nums[i]`.说明同一树层`nums[i - 1]`使用过
*   `used[i - 1] == true`: 说明同一树枝`nums[i - 1]`使用过

但其实, 去重判定条件是`if (i > 0 && nums[i] == nums[i - 1] && used[i - 1] == true)`**也是OK的**

*   `used[i - 1] == false` : **树层**上去重
*   `used[i - 1] == true`: **树枝**上去重

**树层上对前一位去重非常彻底，效率很高，树枝上对前一位去重虽然最后可以得到答案，但是会做很多无用搜索。**

### [剑指 Offer II 085. 生成匹配的括号](https://leetcode.cn/problems/IDBivT/)

#### 题目

> 正整数 `n` 代表生成括号的对数，请设计一个函数，用于能够生成所有可能的并且 \*\*有效的 \*\*括号组合。

**示例 1：**

```
输入：n = 3
输出：["((()))","(()())","(())()","()(())","()()()"]

```

**示例 2：**

```
输入：n = 1
输出：["()"]

```

**提示：**

*   `1 <= n <= 8`

#### 解法

![image.png](https://note.youdao.com/yws/res/10656/WEBRESOURCEdea9fbbc208a33e7d2c138009c1d23c7)

##### 回溯+栈（暴力)

1.  回溯穷举所有的可能性
2.  然后在`path`字符串的长度等于`2 * n`时，判断当前字符串是否是有效的括号
3.  如果是有效的括号，将字符串`path`加入结果数组

###### 代码

```cpp
class Solution {
public:
    vector<string> result;
    bool isVaild(string& path) {
        stack<char> st;
        for (auto it : path) {
            if (it == '(')
                st.push(it);
            else {
                if (st.empty()) return false;
                else st.pop();
            } 
        }
        return st.empty();
    }
    void backtrace(int n, string path) {
        if (path.size() == n) {
            cout << "path: " << path << endl;
            if (isVaild(path)) {
                result.push_back(path);
            }
            return;
        }
        backtrace(n, path + "(");
        backtrace(n, path + ")");
    }
    vector<string> generateParenthesis(int n) {
        // 1. 生成所有可能的组合, 判断组合是否有效
        backtrace(n * 2, "(");
        return result;
    }
};
```

###### 复杂度分析

*   时间复杂度：O(n\* 2^2n^) 对于 2^2n^ 个序列中的每一个, 建立和验证的复杂度为O(n)
*   空间复杂度:  *O*(*n*)

##### &#x20;优化

由图可知，当`path`字符串建立过程中出现了序列无效，就应该停止纵向遍历(递归)

哪些情况是建立过程中知道了序列无效:

1.  右括号的数量 > 左括号的数量
2.  左括号的数量 > `n`

因此, 我们应该跟踪左右括号的数量

```cpp
class Solution {
public:
    vector<string> result;
    void backtrace(int n, string path, int left, int right) {
        if (right > left || left > n) return;
        if (path.size() == 2 * n) {
            result.push_back(path);
            return;
        }
        backtrace(n, path + "(", left + 1, right);
        backtrace(n, path + ")", left , right + 1);
    }
    vector<string> generateParenthesis(int n) {
        // 2. 生成过程中看组合是否有效
        backtrace(n, "(", 1, 0);
        return result;
    }
};
```

### [剑指 Offer II 086. 分割回文子字符串](https://leetcode.cn/problems/M99OJA/)

#### 题目

> 给定一个字符串 s ，请将 s 分割成一些子串，使每个子串都是 回文串 ，返回 s 所有可能的分割方案。
> 回文串 是正着读和反着读都一样的字符串。

**示例 1：**

```
输入：s = "google"
输出：[["g","o","o","g","l","e"],["g","oo","g","l","e"],["goog","l","e"]]

```

**示例 2：**

```
输入：s = "aab"
输出：[["a","a","b"],["aa","b"]]

```

**示例 3：**

    输入：s = "a"
    输出：[["a"]]

 

**提示：**

*   `1 <= s.length <= 16`
*   `s `仅由小写英文字母组成

#### 解法

![image.png](https://note.youdao.com/yws/res/10827/WEBRESOURCE5e898babae769bd025fff7234737f0db)

##### 回溯+回文串判定(暴力)

1.  回溯穷举所有的可能性
2.  然后在每次选中一个子串时，判断当前字符串是否是回文串
3.  如果是有效的括号，将字符串`str`加入路径数组`path`, 然后递归处理下一个子串
4.  有一个变量记录当前处理到了第一个字符，如果`len` == `s.length()`，说明已经找到了一个子串集，加入到结果数组中

###### 代码

```cpp
class Solution {
public:
    vector<vector<string>> result;
    vector<string> path;
    bool isVaild(string& str) {
        int left = 0, right = str.length() - 1;
        while(left < right) {
            if (str[left] != str[right]) return false;
            left ++;
            right --;
        }
        return true;
    }
    void backtrace(string& s, int index, int len) {
        if (len == s.length()) {
            result.emplace_back(path);
            return;
        }
        for (int i = index; i < s.length(); i ++) {
            auto str = s.substr(index, i - index + 1);
            // cout << "str:" << str << endl;
            if (!isVaild(str)) continue;
            path.emplace_back(str);
            backtrace(s, i + 1, len + str.length());
            path.pop_back();
        }
    }
    vector<vector<string>> partition(string s) {
        backtrace(s, 0, 0);
        return result;
    }
};
```

###### 复杂度分析

*   时间复杂度: O(n \* 2^n^)  如果不算树枝剪枝, 且是n个完全相同的字符串, 那么划分方法有`2^n-1^`个 = O(2^n^), 对于每种划分需要O(n)判断是否满足
*   空间复杂度: O(n^2^) 使用 O(n) 的栈空间以及 O(n) 的用来存储当前字符串分割方法的空间

##### 回溯+回文串判定(优化) (错误答案)

如果当前substr取得的子串长度 > `s.length()`, 且当前这个子串不是回文串, 那么就不需要进行横向遍历了.
![](https://raw.githubusercontent.com/Cris-Cui/ImagesForBlog/master/Blog/20230728233015.png)

```cpp
int x = (s.length() - index) % 2 == 0 ? (s.length() - index) / 2 : (s.length() - index) / 2 + 1;
if (!isVaild(str) && str.length() > x) break;
if (!isVaild(str)) continue;
```

*   目前想不出来更优解

### [剑指 Offer II 087. 复原 IP](https://leetcode.cn/problems/0on3uN/)

#### 题目

给定一个只包含数字的字符串 `s` ，用以表示一个 IP 地址，返回所有可能从 `s` 获得的 \*\*有效 IP 地址 \*\*。你可以按任何顺序返回答案。

**有效 IP 地址** 正好由四个整数（每个整数位于 0 到 255 之间组成，且不能含有前导 `0`），整数之间用 `'.'` 分隔。

例如："0.1.2.201" 和 "192.168.1.1" 是 **有效** IP 地址，但是 "0.011.255.245"、"192.168.1.312" 和 "192.168\@1.1" 是 **无效** IP 地址。

**示例 1：**

```
输入：s = "25525511135"
输出：["255.255.11.135","255.255.111.35"]

```

**示例 2：**

```
输入：s = "0000"
输出：["0.0.0.0"]

```

**示例 3：**

```
输入：s = "1111"
输出：["1.1.1.1"]

```

**示例 4：**

```
输入：s = "010010"
输出：["0.10.0.10","0.100.1.0"]

```

**示例 5：**

```
输入：s = "10203040"
输出：["10.20.30.40","102.0.30.40","10.203.0.40"]

```

 

**提示：**

*   `0 <= s.length <= 3000`
*   `s` 仅由数字组成

#### 解法

##### 回溯

1.  遍历所有可能的组合，判断是否满足题意

    *   每一个点分十进制大于1位数时不含有前导零
    *   每一个点分十进制的值 <= 255
2.  树枝优化

    当前点分十进制段不满足题意时，不需要再深度遍历(递归)了 => `continue`

回溯结束判断条件:

*   存在4个点分十进制段，且用完了所有字符，这时候可以将路径字符串加入到结果数组中。

![image.png](https://note.youdao.com/yws/res/10958/WEBRESOURCE821fa2eedb34987a3e36f6a0fc123313)

###### 代码

```cpp
class Solution {
public:
    vector<string> result;
    void backtrace(string& s, int index, string path, int depth) {
        if (depth > 4) return;
        if (depth == 4 && index == s.length()) {
            result.emplace_back(path);
            return;
        } 
        for (int i = index; i < index + 3 && i < s.length(); i ++) {
            auto str = s.substr(index, i - index + 1);
            if (stoi(str) > 255 || (str.length() > 1 && (str[0] == '0'))) continue;
            if (depth != 3)
                backtrace(s, i + 1, path + s.substr(index, i - index + 1) + ".", depth + 1);
            else 
                backtrace(s, i + 1, path + s.substr(index, i - index + 1), depth + 1);
        }
    }
    vector<string> restoreIpAddresses(string s) {
        backtrace(s, 0, "", 0);
        return result;
    }
};
```

###### 复杂度分析

*   时间复杂度: O(3^4^)，IP地址最多包含4个数字，每个数字最多有3种可能的分割方式，则搜索树的最大深度为4，每个节点最多有3个子节点。
*   空间复杂度: O(n) 递归栈大小

