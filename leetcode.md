
- [:wink: 二分](#wink-二分)
  - [:lollipop: 4. 寻找两个正序数组的中位数](#lollipop-4-寻找两个正序数组的中位数)
  - [:lollipop: 153. 寻找旋转排序数组中的最小值](#lollipop-153-寻找旋转排序数组中的最小值)
  - [:lollipop: 33. 搜索旋转排序数组](#lollipop-33-搜索旋转排序数组)
- [:wink: 栈](#wink-栈)
  - [:lollipop: 155. 最小栈](#lollipop-155-最小栈)
  - [:lollipop: 32. 最长有效括号](#lollipop-32-最长有效括号)
- [:wink: 数学](#wink-数学)
  - [:lollipop: 89. 格雷编码](#lollipop-89-格雷编码)
  - [:lollipop: 169. 多数元素](#lollipop-169-多数元素)
  - [:lollipop: 384. 打乱数组](#lollipop-384-打乱数组)
  - [:lollipop: ](#lollipop-)
- [:wink: 数组](#wink-数组)
  - [:lollipop: 215. 数组中的第K个最大元素](#lollipop-215-数组中的第k个最大元素)
  - [:lollipop: 347. 前 K 个高频元素](#lollipop-347-前-k-个高频元素)
  - [:lollipop: 295. 数据流的中位数](#lollipop-295-数据流的中位数)
  - [:lollipop: 128. 最长连续序列](#lollipop-128-最长连续序列)
  - [:lollipop: ](#lollipop--1)
- [:wink: 贪心](#wink-贪心)
  - [:lollipop: 763. 划分字母区间](#lollipop-763-划分字母区间)
- [:wink: DP](#wink-dp)
  - [:lollipop: 53. 最大子数组和](#lollipop-53-最大子数组和)
  - [:lollipop: 62. 不同路径](#lollipop-62-不同路径)
  - [:lollipop: 64. 最小路径和](#lollipop-64-最小路径和)
  - [:lollipop: 5. 最长回文子串](#lollipop-5-最长回文子串)
  - [:lollipop: 516. 最长回文子序列](#lollipop-516-最长回文子序列)
  - [:lollipop: 1143. 最长公共子序列](#lollipop-1143-最长公共子序列)
  - [:lollipop: 1092. 最短公共超序列](#lollipop-1092-最短公共超序列)
  - [:lollipop: 72. 编辑距离](#lollipop-72-编辑距离)
  - [:lollipop: 834. 树中距离之和](#lollipop-834-树中距离之和)
  - [:lollipop: 310. 最小高度树](#lollipop-310-最小高度树)
  - [:lollipop: 1000. 合并石头的最低成本](#lollipop-1000-合并石头的最低成本)
  - [:lollipop: 2376. 统计特殊整数](#lollipop-2376-统计特殊整数)
  - [:lollipop: 233. 数字 1 的个数](#lollipop-233-数字-1-的个数)
  - [:lollipop: 600. 不含连续1的非负整数](#lollipop-600-不含连续1的非负整数)
  - [:lollipop: 1012. 至少有 1 位重复的数字](#lollipop-1012-至少有-1-位重复的数字)
  - [:lollipop: ](#lollipop--2)
- [:wink: 二叉树](#wink-二叉树)
  - [:lollipop: 112. 路径总和](#lollipop-112-路径总和)
  - [:lollipop: ](#lollipop--3)
- [:wink: 图](#wink-图)
  - [:lollipop: 785. 判断二分图](#lollipop-785-判断二分图)
  - [:lollipop: 399. 除法求值](#lollipop-399-除法求值)
  - [:lollipop: ](#lollipop--4)
- [:wink:](#wink)
  - [:lollipop: ](#lollipop--5)

# :wink: 二分

## :lollipop: [4. 寻找两个正序数组的中位数](https://leetcode.cn/problems/median-of-two-sorted-arrays/description/)

- :cherry_blossom: 思路

求中位数，其实就是求第 k 小数的一种特殊情况
由于数列是有序的，我们可以直接一半一半的排除元素，即可以每次循环排除掉 k / 2 个数

- :one: 比较两个数组的第 `k / 2` 个数，不妨设 `A[k / 2 - 1] <= B[k / 2 - 1]`，那么对于 `A[k / 2 - 1]` 而言，最多最多只有 `k / 2 - 1 + k /2 - 1 = k - 2` 个数字比他小，所以 `A[k / 2 - 1]` 最大最大也只能是第 k - 1 小的数，可以直接排除 `0 ~ k / 2 - 1`
- :two: 令 `A = [k / 2 : ], k = k - len(A[ :k / 2])`，在 A 跟 B 中重新找第 k 小的数字，...

***示意图***

![1](https://pic.leetcode-cn.com/735ea8129ab5b56b7058c6286217fa4bb5f8a198e4c8b2172fe0f75b29a966cd-image.png)

![2](https://pic.leetcode-cn.com/09b8649cd2b8bbea74f7f632b098fed5f8404530ff44b5a0b54a360b3cf7dd8f-image.png)

![3](https://pic.leetcode-cn.com/f2d72fd3dff109ad810895b9a0c8d8782f47df6b2f24f9de72704961bc547fcb-image.png)

![4](https://pic.leetcode-cn.com/3c89a8ea29f2e19057b57242c8bc37c5f09b6796b96c30f3d42caea21c12f294-image.png)

***边界条件***

1. 当 k = 1 时直接返回更小的那个数就可以了
2. 若 `k / 2 >= A.size()`，比较 `A.back()` 和 `B[k / 2]`。若 `A.back() <= B[k / 2]`，那么数组 A 就会被全部排除，此时直接定位 B 中第 `k - A.size()` 小的元素即可

- **:beers: 代码**

注意找第 k 小个元素和真实下标之间的关系（nums[k] 为第 k + 1 小的元素）

>c++

```c++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size(), m = nums2.size();

        function<int(int)> getk = [&](int k) -> int {
            int s1 = 0, s2 = 0;
            while (true) {
                if (s1 == n) return nums2[s2 + k - 1];
                else if (s2 == m) return nums1[s1 + k - 1];
                else if (k == 1) return min(nums1[s1], nums2[s2]);
                int e1 = min(s1 + k / 2 - 1, n - 1), e2 = min(s2 + k / 2 - 1, m - 1);
                cout << s1 << "-" << e1 << ", " << s2 << "-" << e2 << endl;
                if (nums1[e1] <= nums2[e2]) {
                    k -= e1 - s1 + 1;
                    s1 = e1 + 1;
                } else {
                    k -= e2 - s2 + 1;
                    s2 = e2 + 1;
                }
            }
        };

        // 找第 k 小的元素，为奇数找第 (n + m + 1) / 2 小的数
        // 偶数找第 (n + m) / 2 和 (n + m) / 2 + 1 小的数
        if ((n + m) & 1) {
            return getk((n + m + 1) / 2);
        } else {
            return (getk((n + m) / 2) + getk((n + m) / 2 + 1)) / 2.0;
        }
    }
};
```

> Go

```go
func findMedianSortedArrays(nums1 []int, nums2 []int) float64 {
    n, m := len(nums1), len(nums2)
    var getK func(int) int
    getK = func(k int) int {
        s1, s2 := 0, 0
        for {
            if s1 == n {
                return nums2[s2 + k - 1]
            } else if s2 == m {
                return nums1[s1 + k - 1]
            } else if k == 1 {
                return min(nums1[s1], nums2[s2])
            }
            e1, e2 := min(s1 + k / 2 - 1, n - 1), min(s2 + k / 2 - 1, m - 1)
            if nums1[e1] <= nums2[e2] {
                k -= e1 - s1 + 1
                s1 = e1 + 1
            } else {
                k -= e2 - s2 + 1
                s2 = e2 + 1
            }
        }
    }
    if (n + m) & 1 == 1 {
        return float64(getK((n + m + 1) / 2))
    } else {
        return float64(getK((n + m) / 2) + getK((n + m) / 2 + 1)) / 2.0
    }
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

## :lollipop: [153. 寻找旋转排序数组中的最小值](https://leetcode.cn/problems/find-minimum-in-rotated-sorted-array/description/)

- :cherry_blossom: 思路

染色法，定义最小值左侧的元素染红色，最小值及最小值右侧的元素染蓝色
比较当前元素跟最右侧元素，若当前元素小于 `nums.back()` 则染蓝色。最右侧元素必定是蓝色，因此从未染色区间[0, n - 2]开始二分

- **:beers: 代码**

注意判断那里若写成 `nums[mid] >= nums.back()`，就可能越界（l 可能等于 n）

> c++

```c++
class Solution {
public:
    int findMin(vector<int>& nums) {
        int n = nums.size(), l = 0, r = n - 2;
        while (l <= r)
        {
            int mid = l + ((r - l) >> 1);
            if (nums[mid] > nums.back()) l = mid + 1;
            else r = mid - 1;
        }
        return nums[l];
    }
};
```

> Go

```go
func findMin(nums []int) int {
    l, r, val := 0, len(nums) - 2, nums[len(nums) - 1]
    for l <= r {
        mid := l + ((r - l) >> 1)
        if nums[mid] > val {
            l = mid + 1
        } else {
            r = mid - 1
        }
    }
    return nums[l]
}
```

## :lollipop: [33. 搜索旋转排序数组](https://leetcode.cn/problems/search-in-rotated-sorted-array/description/?envType=study-plan-v2&envId=top-100-liked)

- :cherry_blossom: 思路

染色法，定义 target 左侧的元素染红色， target 及 target 右侧的元素染蓝色

- case1: nums[i] 在左侧连续子数组上 `(nums[i] > nums.back())`
    - target 在左侧 `(target > nums.back())`，若 `target < nums[mid]` 减小 r；否则增大 l
    - target 在右侧 `(target <= nums.back())`，增大 l
- case1: nums[i] 在右侧连续子数组上 `(nums[i] < nums.back())`
    - target 在右侧 `(target <= nums.back())`，若 `target < nums[mid]` 减小 r；否则增大 l
    - target 在左侧 `(target > nums.back())`，减小 r

- **:beers: 代码**

上述逻辑可以进一步简化，仅考虑染一种颜色的情况，比如仅考虑染蓝色

- :one: nums[i] 在左侧连续子数组上: `target > nums.back() && target < nums[mid]`
- :two: nums[i] 在右侧连续子数组上: `(target > nums.back() || (target <= nums.back() && target < nums[mid]))`

存在重复条件，精简后为

- :one: nums[i] 在左侧连续子数组上: `target > nums.back() && target < nums[mid]`
- :two: nums[i] 在右侧连续子数组上: `target > nums.back() || target < nums[mid]`

> c++

```c++
// 写法 1
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int n = nums.size(), l = 0, r = n - 1;
        while (l <= r) {
            int mid = l + ((r - l) >> 1);
            if (nums[mid] == target) return mid;
            else if (nums[mid] > nums.back()) {
                if (target > nums.back()) {
                    if (target < nums[mid]) r = mid - 1;
                    else l = mid + 1;
                } else l = mid + 1;
            } else {
                if (target <= nums.back()) {
                    if (target < nums[mid]) r = mid - 1;
                    else l = mid + 1;
                } else r = mid - 1;
            }
        }
        return -1;
    }
};

// 写法2
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int n = nums.size(), l = 0, r = n - 1;

        auto isBlue = [&](int mid) -> bool {
            if (nums[mid] > nums.back()) return target > nums.back() && target < nums[mid];
            else return target > nums.back() || target < nums[mid];
        };

        while (l <= r) {
            int mid = l + ((r - l) >> 1);
            if (nums[mid] == target) return mid;
            else if (isBlue(mid)) r = mid - 1;
            else l = mid + 1;
        }
        return -1;
    }
};
```

> Go

```go
func search(nums []int, target int) int {
    val, l, r := nums[len(nums) - 1], 0, len(nums) - 1
    var isBlue func(int) bool
    isBlue = func(mid int) bool {
        if nums[mid] > val {
            return target > val && target < nums[mid]
        } else {
            return target > val || target < nums[mid]
        }
    }
    for l <= r {
        mid := l + ((r - l) >> 1)
        if (nums[mid] == target) {
            return mid
        } else if (isBlue(mid)) {
            r = mid - 1
        } else {
            l = mid + 1
        }
    }
    return -1
}
```

# :wink: 栈

## :lollipop: [155. 最小栈](https://leetcode.cn/problems/min-stack/description/)

- :cherry_blossom: 思路

很容易想到的一种方法是使用辅助栈，原来的栈保存原始结果，辅助栈内保存最小值，两个栈同步进行 `push(), pop()` 即可

如何不使用额外辅助栈来求解本题呢？

难点在于 `pop()` 后如何正确的更新最小值！转换思路，栈内保存每个元素在未压入当前元素时与栈中最小值的差值，并额外记录当前栈中元素的最小值。必须是跟原来栈中的元素计算差值是为了 `pop()` 后能够恢复之前的最小值

**注意!栈中记录的是 `diff = val - original_min`, 即当前元素为最小值等价于 `diff <= 0`**

- :one: push()
    - 若栈为空，则当前元素 (val) 必为最小值，压入 0 并令最小值 (mn) 等于 val
    - 栈非空的话，先压入 diff，再更新 mn
- :two: pop(), 注意弹出的元素可能是当前最小值, 需要检查是否更新最小值
    - diff <= 0 (当前元素是最小值且 diff = val(mn) - original_min)，所以 original_min = mn - diff
    - diff > 0, 弹出当前元素并不影响最小值，直接弹出即可
- :three: top(), 同上, 通过判断当前元素是否为最小值来返回栈顶元素
    - diff <= 0 (当前元素是最小值)，可以直接返回 mn
    - diff > 0 (当前元素非最小值且 diff = val - original_min(mn))，返回 diff + mn


- **:beers: 代码**

注意方法2的数据溢出问题

> c++

```c++
// 方法 1
class MinStack {
public:
    stack<int> stk, min_stk;

    MinStack() {
        min_stk.push(INT_MAX);
    }
    
    void push(int val) {
        stk.push(val);
        min_stk.push(min(val, min_stk.top()));
    }
    
    void pop() {
        stk.pop();
        min_stk.pop();
    }
    
    int top() {
        return stk.top();
    }
    
    int getMin() {
        return  min_stk.top();
    }
};

// 方法 2
class MinStack {
public:
    stack<long> stk; 
    long mn;

    MinStack() {

    }
    
    void push(int val) {
        if (stk.empty()) {
            stk.emplace(0);
            mn = val;
        } else {
            stk.emplace(long(val) - mn);
            mn = min(long(val), mn);
        }
    }
    
    void pop() {
        long diff = stk.top();
        stk.pop();
        if (diff <= 0) {
            mn = mn - diff;
        }
    }
    
    int top() {
        long diff = stk.top();
        if (diff <= 0) return (int)mn;
        else return int(mn + diff); 
    }
    
    int getMin() {
        return int(mn);
    }
};
```

> Go

```go
// 方法 1
type MinStack struct {
    stk []int
    min_stk []int
}

func Constructor() MinStack {
    return MinStack{
        stk:[]int{},
        min_stk:[]int{math.MaxInt},
    }
}

func (this *MinStack) Push(val int)  {
    this.stk = append(this.stk, val)
    this.min_stk = append(this.min_stk, min(val, this.min_stk[len(this.min_stk) - 1]))
}

func (this *MinStack) Pop()  {
    this.stk = this.stk[:len(this.stk) - 1]
    this.min_stk = this.min_stk[:len(this.min_stk) - 1]
}

func (this *MinStack) Top() int {
    return this.stk[len(this.stk) - 1]
}

func (this *MinStack) GetMin() int {
    return this.min_stk[len(this.min_stk) - 1]
}

func min(a, b int) int { if a < b { return a }; return b }

// 方法 2

type MinStack struct {
    stk []int
    mn int
}

func Constructor() MinStack {
    return MinStack {
        stk: []int{},
        mn : 0,
    }
}

func (this *MinStack) Push(val int)  {
    if len(this.stk) == 0 {
        this.stk = append(this.stk, 0)
        this.mn = val
    } else {
        this.stk = append(this.stk, val - this.mn)
        this.mn = min(this.mn, val)
    }
}

func (this *MinStack) Pop()  {
    diff := this.stk[len(this.stk) - 1]
    if diff <= 0 {
        this.mn = this.mn - diff
    }
    this.stk = this.stk[:len(this.stk) - 1]
}

func (this *MinStack) Top() int {
    diff := this.stk[len(this.stk) - 1]
    if diff <= 0 {
        return this.mn
    }
    return diff + this.mn
}

func (this *MinStack) GetMin() int {
    return this.mn
}

func min(a, b int) int { if a < b { return a }; return b }

```

## :lollipop: [32. 最长有效括号](https://leetcode.cn/problems/longest-valid-parentheses/description/?envType=study-plan-v2&envId=top-100-liked)

- :cherry_blossom: 思路

方法1：用栈维护括号的匹配关系
- 若为 '(', 下标入栈
- 若为 ')', 弹出栈顶元素(栈顶对应的非 '(' 即 ')' )
  - 若弹出后仍有元素, 则表明之前栈顶为 '(' 并且剩余的元素即为上一次匹配的上一个下标
  - 若弹出后栈变空，则表明之前栈顶为 ')'，直接当前下标入栈 (表示上一次匹配的前一个位置)

方法2：DP

考虑以 i 结尾的最长有效字符串长度, 那么以 '(' 结尾的必然为0, 仅考虑以 ')' 结尾的字符串即可

主要思想：当前的 ')'先跟前面的 '(' 匹配, 若能匹配上再加上 '(' 之前的有效子串

- 若 `s[i] = ')' && s[i - 1] == '('`, 那么 `dp[i] = dp[i - 2] + 2`
- 若 `s[i] = ')' && s[i - 1] == ')'`, 那么就得继续回退, 检查 `s[i - dp[i - 1] - 1] == '(' ?`
    - 若等于的话就表明 `i - dp[i - 1] - 1 ~ i` 形成了一个有效字符串, 再拼接上之前的就可以了 (以 `i - dp[i - 1] - 2` 结尾的有效字符串长度), 即 `dp[i] = dp[i - 1] + dp[i - dp[i - 1] - 2] + 2`

方法3：遍历

维护两个计数器 `left` 和 `right` 保存到目前为止遇到的左括号数目和右括号数目, 当 `left == right` 时表明找到了有效的字符串

- 从左向右遍历, 当 `right > left` 时, 后面的任意字符串都不可能与先前的字符串构成有效字符串, 清空 left 和 right, 重新找有效字符串, 但是这样面对 `((())` 就无法找到最长的有效字符串 (左括号大于右括号的情况), 从右向左再遍历一遍即可
- 从右向左遍历, 当 `left > right` 时, 清空 left 和 right, 重新找有效字符串

从左向右遍历可以确定右括号更多的子串中的最长有效字符串，从右向左则可以确定左括号更多的子串中的最长有效字符串

空间复杂度 O(1)

- **:beers: 代码**

> c++

```c++
// 方法 1, 栈
class Solution {
public:
    int longestValidParentheses(string s) {
        stack<int> stk; 
        stk.emplace(-1);
        int res = 0;
        for (int i = 0; i < s.size(); ++i) {
            if (s[i] == '(') {
                stk.emplace(i);
            } else {
                stk.pop();
                if (stk.empty()) stk.emplace(i);
                else res = max(res, i - stk.top());
            }
        }
        return res;
    }
};

// 方法 2, DP
int longestValidParentheses(string s) {
    int n = s.size(), res = 0;
    vector<int> f(n, 0);
    for (int i = 1; i < n; ++i) {
        if (s[i] == '(') continue;
        if (s[i - 1] == '(') 
            f[i] = (i - 2 >= 0 ? f[i - 2] : 0) + 2;
        else if (i - f[i - 1] - 1 >= 0 && s[i - f[i - 1] - 1] == '(')
            f[i] = f[i - 1] + 2 + (i - f[i - 1] - 2 >= 0 ? f[i - f[i - 1] - 2] : 0);
        res = max(res, f[i]);
    }
    return res;
}

// 方法 3, 遍历
int longestValidParentheses(string s) {
    int n = s.size(), res = 0;
    int left = 0, right = 0;
    for (int i = 0; i < n; ++i) {
        if (s[i] == '(') ++left;
        else ++right;
        if (left == right) res = max(res, 2 * left);
        else if (right > left) left = right = 0;
    }
    left = right = 0;
    for (int i = n - 1; i >= 0; --i) {
        if (s[i] == '(') ++left;
        else ++right;
        if (left == right) res = max(res, 2 * left);
        else if (left > right) left = right = 0;
    }
    return res;
}
```

> Go

```go
// 方法 1, 栈
func longestValidParentheses(s string) int {
    stk := []int{-1}
    res := 0
    for i, val := range s {
        if val == '(' {
            stk = append(stk, i);
        } else {
            stk = stk[:len(stk) - 1]
            if len(stk) > 0 {
                res = max(res, i - stk[len(stk) - 1])
            } else {
                stk = append(stk, i);
            }
        }
    }
    return res;
}

func max(a, b int) int { if a < b {return b}; return a}

// 方法 2, DP
func longestValidParentheses(s string) int {
    n, res := len(s), 0
    f := make([]int, n)
    for i := 1; i < n; i++ {
        if s[i] == '(' {
            continue;
        } else if (s[i - 1] == '(') {
            if i - 2 >= 0 {
                f[i] = f[i - 2] + 2
            } else {
                f[i] = 2
            }
        } else {
            j := i - f[i - 1] - 1
            if j >= 0 && s[j] == '(' {
                if j - 1 >= 0 {
                    f[i] = f[i - 1] + f[j - 1] + 2
                } else {
                    f[i] = f[i - 1] + 2
                }
            }
        }
        res = max(res, f[i])
    }
    return res
}

func max(a, b int) int {if a < b {return b}; return a}

// 方法 3, 遍历
func longestValidParentheses(s string) int {
    n, res := len(s), 0
    left, right := 0, 0
    for i := 0; i < n; i++ {
        if s[i] == '(' {
            left++
        } else {
            right++
        }
        if left == right {
            res = max(res, left * 2)
        } else if left < right {
            left, right = 0, 0
        }
    }
    left, right = 0, 0
    for i := n - 1; i >= 0; i-- {
        if s[i] == '(' {
            left++
        } else {
            right++
        }
        if left == right {
            res = max(res, left * 2)
        } else if left > right {
            left, right = 0, 0
        }
    }
    return res
}

func max(a, b int) int {if a < b {return b}; return a}
```


# :wink: 数学

## :lollipop: [89. 格雷编码](https://leetcode.cn/problems/gray-code/)

- :cherry_blossom: 思路

从对应的n位二进制码字中直接得到n位格雷码码

- :one: 对n位二进制的数字, 从右到左, 以 0 到 n-1 编号
- :two: 如果二进制码字的第 i 位和 i + 1 位相同，则对应的格雷码的第 i 位为0，否则为1（当i+1=n时，二进制码字的第n位被认为是0，即第n-1位不变）

![](https://pic.leetcode-cn.com/1641571805-DExKoS-image.png)

**总结: 第n个格雷码 G(n) = n ^ (n >> 1)**

- **:beers: 代码**

> c++

```c++
class Solution {
public:
    vector<int> grayCode(int n) {
        vector<int> res(1 << n);
        for (int i = 0; i < res.size(); ++i) {
            res[i] = (i >> 1) ^ i;
        }
        return res;
    }
};
```

> Go

```go
func grayCode(n int) []int {
    res := make([]int, 1 << n)
    for i := range res {
        res[i] = (i >> 1) ^ i
    }
    return res
}
```

## :lollipop: [169. 多数元素](https://leetcode.cn/problems/majority-element/)

- :cherry_blossom: 思路

要求时间复杂度 O(n), 空间复杂度 O(1) , 投票算法
可以将众数看作 +1, 负数看作 -1, 那么最终相加一定是正数

算法流程：
1、维护一个众数winner以及它的次数cnt（初始化为 nums[0] 和 1）
2、遍历数组中的元素
    case1：若cnt == 0，winner换成当前元素，cnt重设为1
    case2：若当前数字等于winner，++cnt，否则 --cnt
3、遍历完数组后，winner即为要找的众数

通俗的理解：同归于尽消杀法 

由于多数超过50%, 比如100个数，那么多数至少51个，剩下少数是49个。

1、第一个到来的士兵，直接插上自己阵营的旗帜占领这块高地，此时领主 winner 就是这个阵营的人，现存兵力 count = 1。
2、如果新来的士兵和前一个士兵是同一阵营，则集合起来占领高地，领主 winner 不变，现存兵力 count++；
3、如果新来到的士兵不是同一阵营，则前方阵营派一个士兵和它同归于尽。 此时前方阵营兵力count --。（即使双方都死光，这块高地的旗帜 winner 依然不变，因为已经没有活着的士兵可以去换上自己的新旗帜）
4、当下一个士兵到来，发现前方阵营已经没有兵力，新士兵就成了领主 winner ，现存兵力 count ++。

- **:beers: 代码**

> c++

```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int winner = nums.front(), cnt = 1, n = nums.size();
        for (int i = 1; i < n; ++i) {
            if (!cnt) {
                winner = nums[i];
                cnt = 1;
            }
            else cnt = cnt + (nums[i] == winner ? 1 : -1);
        }
        return winner;
    }
};
```
> Go
```go
func majorityElement(nums []int) int {
    winner, cnt := nums[0], 1
    for i := 1; i < len(nums); i++ {
        if (cnt == 0) {
            winner, cnt = nums[i], 1
        } else if nums[i] == winner {
            cnt++
        } else {
            cnt--
        }
    }
    return winner
}
```
## :lollipop: [384. 打乱数组](https://leetcode.cn/problems/shuffle-an-array/description/)

- :cherry_blossom: 思路

自己不会写

将数组分为乱序区和非乱序区，每次从非乱序区中随机选择一个元素并与非乱序区的第一个元素进行交换，然后扩充乱序区即可

打乱的时间复杂度为 O(n), 因为只需要遍历 n 个元素

- **:beers: 代码**

> c++

```c++
class Solution {
public:
    Solution(vector<int>& nums) : nums(nums), original_nums(nums) { }

    vector<int> reset() {
        return nums = original_nums;
    }

    vector<int> shuffle() {
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            int j = i + rand() % (n - i);
            swap(nums[i], nums[j]);
        }
        return nums;
    }
    vector<int> nums;
    vector<int> original_nums;
};
```

> Go

```go
type Solution struct {
    nums []int
    original_nums []int
}

func Constructor(nums []int) Solution {
    return Solution {
        nums : nums,
        original_nums : append([]int(nil), nums...),
    }
}

func (this *Solution) Reset() []int {
    copy(this.nums, this.original_nums)
    return this.nums
}

func (this *Solution) Shuffle() []int {
    n := len(this.nums)
    for i := range this.nums {
        j := i + rand.Intn(n - i)
        this.nums[i], this.nums[j] = this.nums[j], this.nums[i]
    }
    return this.nums
}
```

## :lollipop: []()

- :cherry_blossom: 思路


- **:beers: 代码**

> c++

```c++

```

> Go

```go

```

# :wink: 数组

## :lollipop: [215. 数组中的第K个最大元素](https://leetcode.cn/problems/kth-largest-element-in-an-array/description/?envType=study-plan-v2&envId=top-100-liked)

- :cherry_blossom: 思路

自己想到的方法为用小根堆，但是时间复杂度为 O(nlogk)

题目要求时间复杂度为 O(n), 考虑快速选择, 虽然最差情况下的时间复杂度为 O(n^2), 但是因为每次是随机选的数做为参照, 所以很难出现这种情况 (每次恰好随机到了数组的最大值或最小值), 快速选择的平均时间复杂度为 O(n)

最好情况下为 n + n / 2 + n / 4 + ... + 1, 即每次排除一半数据, n / 2 + n / 4 + ... + 1 = n (无穷级数, 记住就行), 所以等于 2n

- **:beers: 代码**

> c++

```c++
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        int n = nums.size();
        function<int(int, int)> quickSelection = [&](int l, int r) -> int {
            int i = int(rand() / double(RAND_MAX) * (r - l) + l), j = r;
            swap(nums[i], nums[l]);
            i = l + 1; // 令小于 i 的为 >= t 的元素, 大于 j 的为 <= t 的元素
            while (true) {
                while (i < r && nums[i] >= nums[l]) ++i;
                while (j > l && nums[j] <= nums[l]) --j;
                if (i >= j) break;
                swap(nums[i], nums[j]);
            }
            // 终止时 i 指向从左向右第一个小于 t 的元素; j 指向从右到左第一个大于 t 的元素
            swap(nums[l], nums[j]);
            if (k == j + 1) return nums[j];
            else if (k < j + 1) return quickSelection(l, j - 1);
            else return quickSelection(j + 1, r);
        }; 
        return quickSelection(0, n - 1);
    }
};
```

> Go

```go
func findKthLargest(nums []int, k int) int {
    n := len(nums)
    var quickSelection func(int, int) int
    quickSelection = func(l, r int) int {
        i, j := rand.Intn(r - l + 1) + l, r // 注意这里生成随机数的 API
        nums[i], nums[l] = nums[l], nums[i]
        i = l + 1
        for {
            for i < r && nums[i] >= nums[l] {
                i++
            }
            for j > l && nums[j] <= nums[l] {
                j--
            }
            if i >= j {
                break
            }
            nums[i], nums[j] = nums[j], nums[i]
        }
        nums[l], nums[j] = nums[j], nums[l]
        if k == j + 1 {
            return nums[j]
        } else if k < j + 1 {
            return quickSelection(l, j - 1)
        } else {
            return quickSelection(j + 1, r)
        }
    }
    return quickSelection(0, n - 1)
}
```

## :lollipop: [347. 前 K 个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/description/?envType=study-plan-v2&envId=top-100-liked)

- :cherry_blossom: 思路

自己想到的方法为哈希统计词频，然后排序选前 k 个词频最高的, O(nlogn)

别人的一个比较巧的思路为桶排序, 首先在统计词频的过程中维护最大的词频, 然后开这么多桶, 再遍历一遍哈希将元素放入桶中即可, O(n)

方法3: 统计完词频后转换为求第 k 个最大词频, 用快速选择, 同样 O(n)

- **:beers: 代码**

> c++

```c++
// 桶排序
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> umap;
        int mx = 0;
        for (int &n : nums) mx = max(++umap[n], mx);
        vector<vector<int>> buckets(mx + 1); // mx 个桶
        for (auto &p : umap) buckets[p.second].emplace_back(p.first);
        vector<int> res;
        for (int i = mx; i > 0 && res.size() < k; --i) {
            for (auto &n : buckets[i]) {
                res.emplace_back(n);
                if (res.size() == k) break;
            }
        }
        return res;
    }
};

class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> umap;
        for (int &n : nums) ++umap[n];
        vector<pair<int, int>> arr(umap.begin(), umap.end());
        int n = arr.size();

        function<void(int, int)> quickSelection = [&](int l, int r) {
            int i = int(rand() / double(RAND_MAX) * (r - l)) + l, j = r;
            swap(arr[l], arr[i]);
            i = l + 1;
            while (true) {
                while (i < r && arr[i].second >= arr[l].second) ++i;
                while (j > l && arr[j].second <= arr[l].second) --j;
                if (i >= j) break;
                swap(arr[i], arr[j]);
            }
            swap(arr[j], arr[l]);
            if (j == k - 1) return;
            else if (j < k - 1) quickSelection(j + 1, r);
            else quickSelection(l, j - 1);
        };

        quickSelection(0, n - 1);
        vector<int> res(k);
        for (int i = 0; i < k; ++i) res[i] = arr[i].first;
        return res;
    }
};
```

> Go

```go
// 桶排序
func topKFrequent(nums []int, k int) []int {
    umap := map[int]int{}
    mx := 0
    for _, val := range nums {
        umap[val]++
        mx = max(umap[val], mx)
    }
    buckets := make([][]int, mx + 1)
    for i := 0; i <= mx; i++ {
        buckets[i] = []int{}
    }
    for val, cnt := range umap {
        buckets[cnt] = append(buckets[cnt], val)
    }
    res := []int{}
    for i := mx; i > 0 && len(res) < k; i-- {
        for _, val := range buckets[i] {
            res = append(res, val);
            if len(res) == k {
                break;
            }
        }
    }
    return res
}

func max(a, b int) int { if a < b {return b}; return a}

// 快速选择
func topKFrequent(nums []int, k int) []int {
    umap := map[int]int{}
    for _, val := range nums {
        umap[val]++
    }
    arr := [][]int{}
    for num, cnt := range umap {
        arr = append(arr, []int{num, cnt})
    }

    var quickSelection func(int, int) 
    quickSelection = func(l, r int) {
        i, j := rand.Intn(r - l + 1) + l, r
        arr[i], arr[l] = arr[l], arr[i]
        i = l + 1
        for {
            for i < r && arr[i][1] >= arr[l][1] {
                i++
            }
            for j > l && arr[j][1] <= arr[l][1] {
                j--
            }
            if i >= j {
                break
            } else {
                arr[i], arr[j] = arr[j], arr[i]
            }
        }
        arr[l], arr[j] = arr[j], arr[l]
        if j == k - 1 {
            return
        } else if j < k - 1 {
            quickSelection(j + 1, r)
        } else {
            quickSelection(l , j - 1)
        }
    }

    quickSelection(0, len(arr) - 1)
    res := make([]int, k)
    for i := 0; i < k; i++ {
        res[i] = arr[i][0]
    }
    return res
}
```


## :lollipop: [295. 数据流的中位数](https://leetcode.cn/problems/find-median-from-data-stream/?envType=study-plan-v2&envId=top-100-liked)

- :cherry_blossom: 思路

为了能在 O(1) 的复杂度内取得当前的中位数, 必须将当前已知的数据流划分为两个区间, 并支持动态调整以保证左区间中的数据 <= 右区间中的数据, 考虑堆

当有新数据到达时为了调整区间内包含的数据，需要知道左区间中的最大值以及右区间中的最小值, 因此定义 l 为大根堆, r 为小根堆; 那么中位数为 `l.top()` 或 `(l.top() + r.top()) / 2.0`

注意在加入新元素时需要保证左区间中的所有元素都 <= 右区间中的所有元素，这样才能保证算出来的中位数是正确的

- 插入前两个区间中的元素相等, 那么插入后变为奇数, 中位数为 `l.top()` (应该把 min(r.top(), num) 插入 l)
  - r 为空, 直接加入 l 即可
  - `num >= r.top()`, 那么 num 应该在右区间, 为了确保插入后 l 的元素更多, 应先将 `r.top()` 弹出并放到 l 中, 再将 num 放到 r 中
  - `num < r.top()`, 那么 num 就应该在左区间, 直接加入 l 即可
- 插入前不等 (`l.size() = r.size() + 1`), 插入后相等, 中位数为 `(l.top() + r.top()) / 2.0` (应该把 max(l.top(), num) 插入 r)
  - `num >= l.top()`, 那么 num 应该在右区间, 直接加入 r 即可
  - `num < l.top`, 那么 num 就应该在左区间, 为了确保插入后元素相等, 应先将 `l.top()` 弹出并放到 r 中, 再将 num 放到 l 中

- **:beers: 代码**

> c++

```c++
class MedianFinder {
public:
    MedianFinder() {

    }
    void addNum(int num) {
        if (left.size() == right.size()) {
            if (right.empty() || num < right.top()) {
                left.emplace(num);
            } else {
                left.emplace(right.top());
                right.pop();
                right.emplace(num);
            }
        } else {
            if (num >= left.top()) {
                right.emplace(num);
            } else {
                right.emplace(left.top());
                left.pop();
                left.emplace(num);
            }
        }
    }

    double findMedian() {
        if (left.size() == right.size()) {
            return (left.top() + right.top()) / 2.0;
        }  else {
            return left.top();
        }
    }
    priority_queue<int, vector<int>, less<int>> left;
    priority_queue<int, vector<int>, greater<int>> right;
};

```

> Go

```go
type MaxHeap struct {
    data []int
    size int
}

func NewMaxHeap() *MaxHeap {
    return &MaxHeap {
        data : make([]int, 0),
        size : 0,
    }
}

func (h *MaxHeap) Emplace(val int) {
    h.data = append(h.data, val)
    h.size++
    h.up(h.size - 1)
}


func (h *MaxHeap) Pop() int {
    if h.size == 0 {
        return -1
    }
    val := h.data[0]
    h.data[0] = h.data[h.size - 1]
    h.data = h.data[:h.size - 1]
    h.size--
    h.down(0)
    return val
}

func (h *MaxHeap) up(index int) {
    for {
        if index == 0 {
            break
        }
        p := (index - 1) / 2
        if h.data[p] >= h.data[index] {
            break;
        }
        h.data[p], h.data[index] = h.data[index], h.data[p]
        index = p
    }
}

func (h *MaxHeap) down(index int) {
    for {
        left := 2 * index + 1
        if left >= h.size {
            break
        }
        right := left + 1
        mx := left
        if right < h.size && h.data[right] > h.data[left] {
            mx = right
        }
        if h.data[index] >= h.data[mx] {
            break
        }
        h.data[index], h.data[mx] = h.data[mx], h.data[index]
        index = mx
    }
}

type MinHeap struct {
    data []int
    size int
}

func NewMinHeap() *MinHeap {
    return &MinHeap {
        data : make([]int, 0),
        size : 0,
    }
}

func (h *MinHeap) Emplace(val int) {
    h.data = append(h.data, val)
    h.size++
    h.up(h.size - 1)
}


func (h *MinHeap) Pop() int {
    if h.size == 0 {
        return -1
    }
    val := h.data[0]
    h.data[0] = h.data[h.size - 1]
    h.size--
    h.data = h.data[:h.size]
    h.down(0)
    return val
}

func (h *MinHeap) up(index int) {
    for {
        if index == 0 {
            break
        }
        p := (index - 1) / 2
        if h.data[p] <= h.data[index] {
            break
        }
        h.data[index], h.data[p] = h.data[p], h.data[index]
        index = p
    }
}

func (h *MinHeap) down(index int) {
    for {
        left := index * 2 + 1
        if left >= h.size {
            break
        }
        right := left + 1
        mn := left
        if right < h.size && h.data[right] < h.data[left] {
            mn = right
        }
        if h.data[index] <= h.data[mn] {
            break
        }
        h.data[index], h.data[mn] = h.data[mn], h.data[index]
        index = mn
    }
}

type MedianFinder struct {
    left *MaxHeap
    right *MinHeap
}


func Constructor() MedianFinder {
    return MedianFinder {
        left: NewMaxHeap(),
        right: NewMinHeap(),
    }
}


func (this *MedianFinder) AddNum(num int)  {
    if this.left.size == this.right.size {
        if this.right.size == 0 || num < this.right.data[0] {
            this.left.Emplace(num)
        } else {
            this.left.Emplace(this.right.Pop())
            this.right.Emplace(num)
        }
    } else {
        if this.left.data[0] <= num {
            this.right.Emplace(num)
        } else {
            this.right.Emplace(this.left.Pop())
            this.left.Emplace(num)
        }
    }
}


func (this *MedianFinder) FindMedian() float64 {
    if this.left.size == this.right.size {
        return float64(this.left.data[0] + this.right.data[0]) / 2
    } else {
        return float64(this.left.data[0])
    }
}
```

## :lollipop: [128. 最长连续序列](https://leetcode.cn/problems/longest-consecutive-sequence/description/)

- :cherry_blossom: 思路

暴力做法为枚举每个数做为起点, 在遍历整个数组找比它大 1 的数, O(n^2), 包含很多重复计算。如当前已经确定了连续序列 `1, 2, 3, 4, 5`, 下次遍历到 2 的时候又来一遍, `2, 3, 4, 5` , 无用功

自己只想到了先排序再类似滑动窗口划过去, 当当前边界的值跟之前的值相等或者当前右边界的值恰好比上一个大1时移动右边界, 最后右边界的值减左边界的值 + 1 即为这个连续序列的长度, 但是要排序, O(nlogn)

要想时间复杂度达到 O(n), 需要即不能重复计算, 又得在 O(1) 的时间内找到下一个更大的数; 这个思路很巧, 就是用哈希, 它能在 O(1) 的时间内找到是否存在下一个更大的元素, 为了避免重复计算, 当且仅当这个数字为第一个数字时才计算它的长度 (即不存在 num - 1 这个数), 那么每个数字最多遍历两遍, O(n)

- **:beers: 代码**

> c++

```c++
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<int> uset;
        for (int &num : nums) uset.emplace(num);
        int res = 0;
        for (const int &num : nums) {
            if (!uset.count(num - 1)) {
                int next = num;
                while (uset.count(next)) ++next;
                res = max(res, next - num);
            }
        }
        return res;
    }
};
```

> Go

```go
func longestConsecutive(nums []int) int {
    uset := map[int]bool{}
    for _, num := range nums {
        uset[num] = true
    }
    res := 0
    for _, num := range nums {
        if !uset[num - 1] {
            next := num + 1
            for uset[next] {
                next++
            }
            res = max(res, next - num)
        }
    }
    return res
}

func max(a, b int)int {if a < b {return b}; return a}
```

## :lollipop: []()

- :cherry_blossom: 思路


- **:beers: 代码**

> c++

```c++

```

> Go

```go

```

# :wink: 贪心

## :lollipop: [763. 划分字母区间](https://leetcode.cn/problems/partition-labels/?envType=study-plan-v2&envId=top-100-liked)

- :cherry_blossom: 思路

自己只是想到了统计词频，不断向外扩并减少词频，只要选择范围内的所有字符的词频为 0 就选

方法1：但是可以直接统计每个字符的最右侧位置 (表示要选当前字符最少需要包括的范围)，然后类似于跳跃区间，每次走到不能走为止就可以了

方法2：类似于上面，纪录每个字符的左右区间，转换题目为求最大非重叠区间问题 (按左端点排序, 只要当前当前左端点大于已知最远右端点说明上一个重叠区间已经全部找到)

- **:beers: 代码**

> c++

```c++
class Solution {
public:
    vector<int> partitionLabels(string s) {
        int n = s.size();
        vector<int> loc(26, 0);
        for (int i = 0; i < n; ++i)
            loc[s[i] - 'a'] = i;
        vector<int> res;
        for (int i = 0, mx = 0, last = -1; i < n; ++i) {
            mx = max(mx, loc[s[i] - 'a']);
            if (mx == i) {
                res.emplace_back(i - last);
                last = i;
            }
        }
        return res;
    }
};

class Solution {
public:
    vector<int> partitionLabels(string s) {
        int n = s.size();
        vector<vector<int>> loc(27, {n + 1, -1});
        for (int i = 0; i < n; ++i) {
            loc[s[i] - 'a'][0] = min(loc[s[i] - 'a'][0], i);
            loc[s[i] - 'a'][1] = max(loc[s[i] - 'a'][1], i);
        }
        sort(loc.begin(), loc.end());
        vector<int> res;
        for (int i = 1, s = -1, e = loc[0][1]; i < 27 && e >= 0; ++i) {
            if (loc[i][0] > e) { // 找到了下一个区间
                res.emplace_back(e - s);
                s = e;
                e = loc[i][1];
                continue;
            }
            e = max(e, loc[i][1]);
        }
        return res;
    }
};
```

> Go

```go
func partitionLabels(s string) []int {
    loc := make([]int, 26)
    for i, c := range s {
        loc[c - 'a'] = max(loc[c - 'a'], i)
    }
    res := []int{}
    start, mx := -1, 0
    for i, c := range s {
        mx = max(mx, loc[c - 'a'])
        if mx == i {
            res = append(res, i - start)
            start = i
        }
    }
    return res
}

func max(a, b int) int { if a < b {return b}; return a}
```

# :wink: DP

## :lollipop: [53. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/description/)

- :cherry_blossom: 思路

- 思路一、DP

计算以每个元素结尾的情况下的最大子数组和, 因为子数组长度至少为 1, 所以分为要前面的子数组和不要前面的子数组两种情况 `sum[i] = max(nums[i], nums[i] + sum[i - 1])`

- 思路二、贪心

计算累加和, 若累加和小于0, 清空 (后面元素必然不要这个累加和更优)

- **:beers: 代码**

> c++

```c++
// DP
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int n = nums.size(), res = nums[0];
        for (int i = 1; i < n; ++i) {
            nums[i] = max(nums[i], nums[i] + nums[i - 1]);
            res = max(res, nums[i]);
        }
        return res;
    }
};

// 贪心

class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int res = nums[0], sum = 0;
        for (int &num : nums) {
            sum += num;
            res = max(res, sum);
            sum = max(sum, 0);
        }
        return res;
    }
};
```

> Go

```go
// DP
func maxSubArray(nums []int) int {
    res := nums[0]
    for i := 1; i < len(nums); i++ {
        nums[i] = max(nums[i], nums[i] + nums[i - 1])
        res = max(res, nums[i])
    }
    return res
}

func max(a, b int) int { if a < b {return b}; return a}

// 贪心
func maxSubArray(nums []int) int {
    res, sum := nums[0], 0
    for _, val := range nums {
        sum += val
        res = max(res, sum)
        sum = max(sum, 0)
    }
    return res
}

func max(a, b int) int { if a < b {return b}; return a}
```

## :lollipop: [62. 不同路径](https://leetcode.cn/problems/unique-paths/description/?envType=study-plan-v2&envId=top-100-liked)

- :cherry_blossom: 思路

常规 DP 比较简单，但是还有一种思路是看成组合问题

因为必定要走 n + m - 2 步，等价于求组合数 (n + m - 2 里选 n - 1 个向下走)

m 个里面选 n 个的组合数 `C(m, n)= m! / (n! * (m - n)!)`, 计算阶乘可能会溢出
可以用 `C(m, n) = C(m - 1, n) + C(m - 1, n - 1)`

用记忆化可以通过, 但是不能改为 DP, 因为 DP 要计算所有状态, 中间某个状态会溢出


## :lollipop: [64. 最小路径和](https://leetcode.cn/problems/minimum-path-sum/description/?envType=study-plan-v2&envId=top-100-liked)

- :cherry_blossom: 思路

记忆化很好写出来, 不过自己改 DP 改了很久, 总是有问题

这题跟之前那些记忆化改 DP 的区别在于数据是二维的, 因此边界条件不止只有一个 (之前的可能 i <= 0 就可以返回结果了, 但是二维的就 j 也可能越界, 边界条件更加复杂化)

自己的 dfs 写法是不管对不对先进入, 进入后再判断条件, 但是它等价于在进入前判断, 即不会进入非法状态, 因此可以按照这个思路改

此外, 也可以直接初始化一行, 一列而不是单个元素 (个人更加喜欢这种写法, 因为 dfs 更加喜欢先调用再判断而不是先判断再调用)

- **:beers: 代码**

> c++

```c++
// 记忆化
int minPathSum(vector<vector<int>>& grid) {
    int m = grid.size(), n = grid[0].size();
    vector<vector<int>> cache(m, vector<int>(n, -1));
    function<int(int, int)> dfs = [&](int i, int j) -> int {
        if (i == 0 && j == 0) return grid[i][j];
        if (i < 0 || j < 0) return INT_MAX / 2;
        if (cache[i][j] != -1) return cache[i][j];
        return cache[i][j] = min(dfs(i - 1, j), dfs(i, j - 1)) + grid[i][j];
    };
    return dfs(m - 1, n  -1);
}

// 改法 1, 等价于将 dfs 中判断非法的条件移到后面向下调用 dfs 的过程中了
int minPathSum(vector<vector<int>>& grid) {
    int m = grid.size(), n = grid[0].size();
    vector<vector<int>> f(m, vector<int>(n, INT_MAX));
    f[0][0] = grid[0][0];
    for (int i = 0; i < m; ++i) {
        for (int j = 0; j < n; ++j) {
            if (i > 0 && j > 0) f[i][j] = min(f[i - 1][j], f[i][j - 1]) + grid[i][j];
            else if (i > 0) f[i][j] = f[i - 1][j] + grid[i][j];
            else if (j > 0) f[i][j] = f[i][j - 1] + grid[i][j];
        }
    }
    return f[m - 1][n - 1];
}

// 改法 2, 初始化一行元素, 之后从第一行开始遍历

// 错误的代码, 仅初始化了一个元素, 仍从第 0 行开始遍历
// 问题在于 f[1][1] 仍然会计算一次, 并且等于 (INT_MAX >> 1) + grid[0][0]
int minPathSum(vector<vector<int>>& grid) {
    int m = grid.size(), n = grid[0].size();
    vector<vector<int>> f(m + 1, vector<int>(n + 1, INT_MAX / 2));
    f[1][1] = grid[0][0];
    for (int i = 0; i < m; ++i) {
        for (int j = 0; j < n; ++j) {
            f[i + 1][j + 1] = min(f[i][j + 1], f[i + 1][j]) + grid[i][j];
        }
    }
    return f[m][n];
}

// 正确的代码, 初始化一行, 从第一行开始处理
int minPathSum(vector<vector<int>>& grid) {
    int m = grid.size(), n = grid[0].size();
    vector<vector<int>> f(m + 1, vector<int>(n + 1, INT_MAX / 2));
    f[1][1] = grid[0][0];
    for (int j = 1; j < n; ++j) f[1][j + 1] = grid[0][j] +  f[1][j];
    for (int i = 1; i < m; ++i) {
        for (int j = 0; j < n; ++j) {
            f[i + 1][j + 1] = min(f[i][j + 1], f[i + 1][j]) + grid[i][j];
        }
    }
    return f[m][n];
}

// 空间优化
int minPathSum(vector<vector<int>>& grid) {
    int m = grid.size(), n = grid[0].size();
    vector<int> f(n + 1, INT_MAX / 2);
    f[1] = grid[0][0];
    for (int j = 1; j < n; ++j) f[j + 1] = grid[0][j] + f[j];
    for (int i = 1; i < m; ++i) {
        for (int j = 0; j < n; ++j) {
            f[j + 1] = min(f[j + 1], f[j]) + grid[i][j];
        }
    }
    return f[n];
}
```

> Go

```go
func minPathSum(grid [][]int) int {
    m, n := len(grid), len(grid[0])
    f := make([]int, n + 1)
    f[0], f[1] = math.MaxInt, grid[0][0]
    for j := 1; j < n; j++ {
        f[j + 1] = grid[0][j] + f[j]
    }
    for i := 1; i < m; i++ {
        for j := 0; j < n; j++ {
            f[j + 1] = grid[i][j] + min(f[j + 1], f[j])
        }
    }
    return f[n]
}

func min(a, b int) int {if a < b {return a}; return b}
```

## :lollipop: [5. 最长回文子串](https://leetcode.cn/problems/longest-palindromic-substring/description/?envType=study-plan-v2&envId=top-100-liked)

- :cherry_blossom: 思路

**方法一、区间DP**

自己拿的区间 DP 的思想写的, 因为大区间由小区间推导而来, 定义 dp[i][j] 表示 s[i]~s[j] 是否为回文串, 那么 dp[i][j] = s[i] == s[j] && dp[i + 1][j - 1]

时间复杂度 O(n^2)

**方法二、中心扩散法**

回文串必定是从一个字符或两个字符向外扩充得来的，因此可以直接利用双指针不断向外扩直到扩到不满足时为止，记录最长的回文子串

时间复杂度 O(n^2)

**方法三、Manacher**

暴力解法为从某个位开始依次向左向右检测，挨个检测得到最长回文子串。但是这种方式会漏掉偶数的最长回文，如abba，因为它的中心在一个虚的位置，两个b中间，可以考虑填充字符串

- 回文半径和回文直径

回文半径指某个中心点向外扩能扩的最远距离, 回文直径即以 i 为中心的最长回文串的长度

如 `# c # a # b # c # b # a # d #`，字符c的回文半径为6 (`c # b # a #`)，回文直径为11 (`# a # b # c # b # a #`)

- 最右回文右边界和中心点

最右回文右边界和中心点是到目前为止扩充到的最右边界以及它对应的中心点

扩充的过程中检测到的最右回文边界，初始化为-1，中心点同样初始化为-1。那么右边界 R 和中心点 C 同时更新，如检测到 `# c # a # b # c # b # a # d #` 中间的 c 字符时 (扩充的回文0串为 `# a # b # c # b # a #`)，R 为 d 之前的 #，C 为字符 c

在遍历的过程中可以利用之前计算得到的回文半径来加速计算当前的回文半径

- case1: 当前字符在最右回文右边界之外

    - 暴力解法，从当前字符向两侧扩充，计算最长回文子串

- case2: 当前字符在最右回文右边界内部，做 i 关于 C 的对称点 i'

    - case2.1: i' 位置字符的回文区域在L和R内部

        - i 位置字符的的回文半径等于 i' 位置字符的回文半径

    - case2.2: i' 位置字符的回文区域超出了L

        - i 位置字符的的回文半径等于 i 到 R 的距离

    - case2.3: i' 位置字符的回文区域刚好在L上

        - i 位置字符的的回文半径至少等于 i 到 R 的距离，需要继续向 R 右侧进行扩充检验

加速计算每个字符的回文半径后, 时间复杂度为 O(n), 最终最长的回文子串长度即为回文直径除2

**错误思路**

先反转字符串再计算源字符串和反转后的字符串的最长公共子串

因为虽然回文子串反转后相等但是反转后的相等的子串并不一定是回文子串，非充要条件

> "aacabdkacaa"
> "aacakdbacaa"

最长公共子串为 "aaca", 非回文串。最长回文子串的性质要求其是回文的，而最长公共子串的性质要求其是连续的!!

```c++
// 错误思路
string longestPalindrome(string s) {
    int n = s.size();
    string tmp(s);
    reverse(tmp.begin(), tmp.end());
    int lens = 1, start = 0;

    vector<vector<int>> cache(n, vector<int>(n, -1));
    // 计算以 i 和 j 结尾的连续子串长度
    function<int(int, int)> dfs = [&](int i, int j) -> int {
        if (i < 0 || j < 0) return 0;
        if (cache[i][j] != -1) return cache[i][j];
        if (s[i] == tmp[j]) cache[i][j] = dfs(i - 1, j - 1) + 1;
        else cache[i][j] = 0;
        if (cache[i][j] > lens) {
            lens = cache[i][j];
            start = i - lens + 1;
        }
        return cache[i][j];
    };
    for (int i = n - 1; i >= 0; --i) {
        for (int j = n - 1; j >= 0; --j) {
            dfs(i, j);
        }
    }

    return s.substr(start, lens);
}
```

- **:beers: 代码**

> c++

```c++
// 方法一、区间 DP
class Solution {
public:
    string longestPalindrome(string s) {
        int n = s.size();
        vector<vector<bool>> f(n, vector<bool>(n, false));
        f[n - 1][n - 1] = true;
        int l = 0, r = 0;
        for (int i = 0; i < n - 1; ++i) {
            f[i][i] = true;
            f[i][i + 1] = s[i] == s[i + 1];
            if (f[i][i + 1]) {
                l = i, r = i + 1;
            }
        }
        // 注意可以取到 n, wa 了
        for (int len = 3; len <= n; ++len) {
            for (int i = 0, j = i + len - 1; j < n; ++i, ++j) {
                f[i][j] = s[i] == s[j] && f[i + 1][j - 1];
                if (f[i][j]) {
                    l = i, r = j;
                }
            }
        }
        return s.substr(l, r - l + 1);
    }
};

// 方法二、中心扩散法
class Solution {
public:
    string longestPalindrome(string s) {
        int n = s.size();
        int lens = 1, l = 0;
        auto expand = [&](int i, int j) {
            while (i >= 0 && j < n && s[i] == s[j]) {
                --i, ++j;
            }
            // 此时 i + 1 ~ j - 1 为回文串
            if (j - i - 1 > lens) {
                lens = j - i - 1;
                l = i + 1;
            }
        };
        for (int i = 0; i < n; ++i) {
            expand(i, i);
            expand(i, i + 1);
        }
        return s.substr(l, lens);
    }
};

// 方法三、Manacher
class Solution {
public:
    string longestPalindrome(string s) {
        int n = 2 * s.size() + 1;
        string ss(n, '#');
        for (int i = 1; i < ss.size(); i += 2) {
            ss[i] = s[i / 2];
        }
        int lens = -1, index = -1; // 保存最长的回文长度以及中心位置
        int C = -1, R = -1; // C 为回文中心点，R 表示最右回文边界的下一个位置
        vector<int> radius(n, 1); // 每个位置的回文半径
        for (int i = 0; i < n; ++i) {
            // 最少的回文半径，当 i < R 时为两种情况的最小值，否则当 i == R 或 i > R 时设为1
            radius[i] = i < R ? min(radius[C * 2 - i], R - i) : 1;
            // 向两边扩充
            while (i + radius[i] < n && i - radius[i] >= 0) {
                if (ss[i + radius[i]] == ss[i - radius[i]]) ++radius[i];
                else break;
            }
            if (i + radius[i] > R) { // 更新右边界以及中心点
                R = i + radius[i];
                C = i;
            }
            if (radius[i] > lens) {
                lens = radius[i];
                index = i;
            }
        }
        // 原串最大的回文子串长度为半径减 1, 新串的第 i 个位置对应于原串 i / 2
        return s.substr((index - lens + 1) / 2, lens - 1);
    }
};
```

> Go

```go
// 方法一、区间 DP
func longestPalindrome(s string) string {
    n := len(s)
    f := make([][]bool, n)
    l, r := 0, 0
    for i := range f {
        f[i] = make([]bool, n)
        f[i][i] = true
        if i < n - 1 {
            f[i][i + 1] = s[i] == s[i + 1]
            if f[i][i + 1] {
                l, r = i, i + 1
            }
        }
    }
    for lens := 3; lens <= n; lens++ {
        // 注意 Go 没有, 表达式, 因此不能 i++, j++ (i++是一个语句了, 执行完后就必须{, 只能平行赋值)
        for i, j := 0, lens - 1; j < n; i, j = i + 1, j + 1 {
            f[i][j] = s[i] == s[j] && f[i + 1][j - 1]
            if f[i][j] {
                l, r = i, j
            }
        } 
    }
    return s[l: r + 1]
}

// 方法二、中心扩散法
func longestPalindrome(s string) string {
    n, lens, start := len(s), 1, 0
    var expand func(int, int)
    expand = func(l, r int) {
        for l >= 0 && r < n && s[l] == s[r] {
            l--
            r++
        }
        if r - l - 1 > lens {
            lens = r - l - 1
            start = l + 1
        }
    }
    for i := range s {
        expand(i, i)
        expand(i, i + 1)
    }
    return s[start : start + lens]
}

// 方法三、Manacher
func longestPalindrome(s string) string {
    n := 2 * len(s) + 1
    ss := make([]byte, n)
    index := 0
    for i := range ss {
        if i & 1 == 1 {
            ss[i] = s[index]
            index++
        } else {
            ss[i] = '#'
        }
    }
    lens, C, R := -1, -1, -1
    radius := make([]int, n)
    for i := range ss {
        radius[i] = 1
        if i < R {
            radius[i] = min(radius[2*C-i], R - i)
        }
        for i + radius[i] < n && i - radius[i] >= 0 {
            if ss[i + radius[i]] == ss[i - radius[i]] {
                radius[i]++
            } else {
                break
            }
        }
        if i + radius[i] > R {
            R = i + radius[i]
            C = i
        }
        if radius[i] > lens {
            lens = radius[i]
            index = i
        }
    }
    return s[(index - lens + 1) / 2 : (index + lens - 1) / 2]
}

func min(a, b int) int {if a < b {return a}; return b}
```

## :lollipop: [516. 最长回文子序列](https://leetcode.cn/problems/longest-palindromic-subsequence/)

- :cherry_blossom: 思路

自己只想到了回溯构造所有子序列再检查是否为回文的方法, 超时

**方法一、转化为最长公共子序列问题**

将 s 反转后求 s' 和 s 的最长公共子序列 (可以这么转化的根本原因是因为可以要求不连续)。求 s 和 s' 的最长公共子序列时, 选择的字符必定是一一对应的 (同一个位置上的字符相等, 但是 s' 是 s 反转过来的, 所以若对应位置相等, 那么后面的对称位置必定有相应的字符)

> a b c e d c b a
> a b c d e c b a

两个字符串 s, s' 的最长公共子序列必定是对应下标处字符相等的子序列, 即回文自序列

**方法二、区间 DP**

子序列问题, 选或不选的思路

- 子问题: `[i : j]` 的最长回文子序列
- 当前操作
    - 若 `s[i] == s[j]`, 都选
    - 若 `s[i] != s[j]`, 不能都选
- 下一个子问题
    - 若 `s[i] == s[j]`, 计算 `[i + 1 : j - 1]` 的最长回文子序列
    - 若 `s[i] != s[j]`, 计算 `[i + 1 : j]` 和 `[i : j - 1]` 的最长回文子序列

边界条件为 `i == j` 的话为 1; `i > j` 的话为 0

- **:beers: 代码**

> c++

```c++
// 方法一、转化为最长公共子序列问题
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        int n = s.size();
        string ss(s);
        reverse(ss.begin(), ss.end());
        vector<int> f(n + 1, 0);
        for (int i = 0; i < n; ++i) {
            int pre = 0;
            for (int j = 0; j < n; ++j) {
                int tmp = f[j + 1];
                if (s[i] == ss[j]) f[j + 1] = pre + 1;
                else f[j + 1] = max(f[j], f[j + 1]);
                pre = tmp;
            }
        }
        return f[n];
    }
};

// 方法二、区间 DP
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        int n = s.size();
        vector<vector<int>> f(n, vector<int>(n, 0));
        // 灵府的根据 dfs 来改的, i 由大的推小的, j 由小的推大的
        // 所以 i 从大到小, j 从大到小
        for (int i = n - 1; i >= 0; --i) {
            f[i][i] = 1;
            for (int j = i + 1; j < n; ++j) {
                if (s[i] == s[j]) f[i][j] = 2 + f[i + 1][j - 1];
                else f[i][j] = max(f[i + 1][j], f[i][j - 1]);
            }
        }
        return f[0][n - 1];

        // 自己是按照区间长度写的
        // for (int i = 0; i < n; ++i) f[i][i] = 1;
        // for (int lens = 2; lens <= n; ++lens) {
        //     for (int i = 0, j = i + lens - 1; j < n; ++i, ++j) {
        //         if (s[i] == s[j]) f[i][j] = f[i + 1][j - 1] + 2;
        //         else f[i][j] = max(f[i + 1][j], f[i][j - 1]);
        //     }
        // }
        // return f[0][n - 1];

        // function<int(int, int)> dfs = [&](int i, int j) -> int {
        //     if (i == j) return 1;
        //     else if (i > j) return 0;
        //     if (s[i] == s[j]) return dfs(i + 1, j - 1) + 2;
        //     else return max(dfs(i + 1, j), dfs(i, j - 1));
        // };
        // return dfs(0, n - 1);
    }
};

// 空间优化
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        int n = s.size();
        vector<int> f(n, 0);
        // 二维 DP 计算顺序为从右下角不断向上走, 每行从左往右计算, 依赖于左下, 下, 左
        for (int i = n - 1; i >= 0; --i) {
            int pre = 0; //f[i + 1][i]
            f[i] = 1;
            for (int j = i + 1; j < n; ++j) {
                int tmp = f[j];
                if (s[i] == s[j]) f[j] = 2 + pre;
                else f[j] = max(f[j], f[j - 1]);
                pre = tmp;
            }
        }
        return f[n - 1];
    }
};
```

> Go

```go
// 方法一、转化为最长公共子序列问题
func longestPalindromeSubseq(s string) int {
    n := len(s)
    ss := make([]rune, n)
    for i, c := range s {
        ss[n - i - 1] = c
    }
    f := make([]int, n + 1)
    for _, c1 := range s {
        pre := 0
        for j, c2 := range ss {
            tmp := f[j + 1]
            if c1 == c2 {
                f[j + 1] = pre + 1 
            } else {
                f[j + 1] = max(f[j + 1], f[j])
            }
            pre = tmp
        }
    }
    return f[n]
}

func max(a, b int) int { if a < b {return b}; return a}

// 方法二、区间 DP
func longestPalindromeSubseq(s string) int {
    n := len(s)
    f := make([]int, n)
    for i := n - 1; i >= 0; i-- {
        f[i] = 1
        pre := 0
        for j := i + 1; j < n; j++ {
            tmp := f[j]
            if s[i] == s[j] {
                f[j] = 2 + pre
            } else {
                f[j] = max(f[j], f[j - 1])
            }
            pre = tmp
        }
    }
    return f[n - 1]
}

func max(a, b int) int {if a < b {return b}; return a}
```

## :lollipop: [1143. 最长公共子序列](https://leetcode.cn/problems/longest-common-subsequence/description/?envType=study-plan-v2&envId=top-100-liked)

- :cherry_blossom: 思路

子序列问题 (选或不选, 选哪个)
用选或不选的思路, dfs 很好写, 但是自己改成 DP 的空间优化时改的不好, 用了两个一维数组 (当前状态依赖于上一行同位置的状态, 左上方的状态和左边的状态, 因为同时需要左上方和左方, 所以用了两个一维数组)

因为只需要正左上方, 所以没必要保存上一行完整的状态, 用一个临时变量记录即可

- **:beers: 代码**

> c++

```c++
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        int n = text1.size(), m = text2.size();
        vector<int> f(m + 1, 0);
        for (int i = 0; i < n; ++i) {
            int pre = 0;
            for (int j = 0; j < m; ++j) {
                int tmp = f[j + 1];
                if (text1[i] == text2[j]) f[j + 1] = pre + 1;
                else f[j + 1] = max(f[j + 1], f[j]);
                pre = tmp;
            }
        }
        return f[m];
    }
};
```

> Go

```go
func longestCommonSubsequence(text1 string, text2 string) int {
    m := len(text2)
    f := make([]int, m + 1)
    for _, c1 := range text1 {
        pre := 0
        for j, c2 := range text2 {
            tmp := f[j + 1]
            if c1 == c2 {
                f[j + 1] = pre + 1
            } else {
                f[j + 1] = max(f[j], f[j + 1])
            }
            pre = tmp
        }
    }
    return f[m]
}

func max(a, b int) int {if a < b {return b}; return a}
```

## :lollipop: [1092. 最短公共超序列](https://leetcode.cn/problems/shortest-common-supersequence/description/)

- :cherry_blossom: 思路

自己不会写, 本题为求包含两个字符序列的最短序列

**站在答案的角度考虑(考虑选哪个)**
对于 `s=abac` 和 `t=cab`，考虑从后往前构造答案（最短公共超序列）。想一想，答案的最后一个字母是什么？

要么是 s 的最后一个字母 c，要么是 t 的最后一个字母 b。其它字母是没有意义的，假设答案为 `cabaca`，最后一个字母是 a，完全可以把 a 去掉，不会影响 s 和 t 作为答案的子序列

**类似最长公共子序列，分类讨论如下：**

-  s 和 t 的最后一个字母不同：
    - 如果答案的最后一个字母为 s 的最后一个字母，那么问题变成构造 aba 和 cab 的答案，记作 ans1
  
    - 如果答案的最后一个字母为 t 的最后一个字母，那么问题变成构造 abac 和 ca 的答案，记作 ans2

    - 如果 ans1 比 ans2 更短，那么答案就是 ans1 加上 s 的最后一个字母

    - 否则，答案就是 ans2 加上 t 的最后一个字母

- 如果 s 和 t 的最后一个字母相同，那么这个字母就是答案的最后一个字母。问题变成构造 ab 和 c 的答案

**边界：**
- 如果 s 是空串，则答案为 t

- 如果 t 是空串，则答案为 s

虽然有这个思路了但是还是会有各种问题 (代码也很难写), 最后需要 dfs 套 dfs 才能过 (因为涉及到了字符串的构造, 构造也需要时间)

下面的代码都给了时间复杂度和空间复杂度 (灵神的分析太强了)

- **:beers: 代码**

> c++

简单递归以及记忆化
```c++
// 只放了记忆化版本
class Solution {
public:
    string shortestCommonSupersequence(string str1, string str2) {
        int n = str1.size(), m = str2.size();
        vector<vector<string>> cache(n, vector<string>(m, ""));
        function<string(int, int)> dfs = [&](int i, int j) -> string {
            if (i < 0) return str2.substr(0, j + 1);
            if (j < 0) return str1.substr(0, i + 1);
            if (!cache[i][j].empty()) return cache[i][j];
            if (str1[i] == str2[j]) {
                return cache[i][j] = dfs(i - 1, j - 1) + str1[i];
            } else {
                string s1 = dfs(i - 1, j), s2 = dfs(i, j - 1);
                if (s1.size() < s2.size()) return cache[i][j] = s1 + str1[i];
                else return cache[i][j] = s2 + str2[j];
            }
        };
        return dfs(n - 1, m - 1);
    }
};

// 改递推超时
class Solution {
public:
    string shortestCommonSupersequence(string str1, string str2) {
        int n = str1.size(), m = str2.size();
        vector<string> f(m + 1, "");
        for (int j = 0; j < m; ++j) f[j + 1] = str2.substr(0, j + 1);
        for (int i = 0; i < n; ++i) {
            string pre = str1.substr(0, i); 
            f[0] = str1.substr(0, i + 1);
            for (int j = 0; j < m; ++j) {
                string tmp = f[j + 1];
                if (str1[i] == str2[j]) f[j + 1] = pre + str1[i];
                else if (f[j + 1].size() > f[j].size()) {
                    f[j + 1] = f[j] + str2[j]; 
                } else {
                    f[j + 1] = f[j + 1] + str1[i];
                }
                pre = tmp;
            }
        }
        return f[m];
    }
};
```
- 递归

  - 时间复杂度：`O((n + m) * 2^(n + m))`, 其中 n 为 s 的长度，m 为 t 的长度。可以视作在一棵高度为 n+m 的二叉树上递归，每次递归又需要花费 O(n+m) 的时间拼接字符串，所以时间复杂度为 `O((n + m) * 2^(n + m))`

  - 空间复杂度：O(n+m)

- 记忆化
  
  - 时间复杂度：`O(nm(n+m))`。由于每个状态只会计算一次，因此**动态规划的时间复杂度 = 状态个数 × 单个状态的计算时间**。本题的状态个数为 O(nm)，单个状态的计算时间为 O(n+m)（拼接字符串），因此时间复杂度为 `O(nm(n+m))`

  - 空间复杂度：O(nm(n+m))。一共有 O(nm) 个状态，再算上返回值需要 O(n+m) 的空间，所以需要 O(nm(n+m)) 的空间实现记忆化

递归超时，记忆化超内存

**记忆化的进一步优化**

如果只求最短公共超序列的长度，那么递归返回的是一个整数而不是字符串，这样就只需要 O(nm) 的时间和空间，这是可以接受的。问题在于，我们需要构造一个具体的最短公共超序列。

假设 dfs 返回的就是最短公共超序列的长度，我们再回过头来看「初步思路」中的分类讨论：

- `s[i] != t[j]`:
  - 如果 `dfs(i,j) = dfs(i−1,j) + 1`，说明 `dfs(i−1,j) <= dfs(i,j−1)`，答案应为 `dfs(i−1,j)` 对应的最短公共超序列加上 `s[i]`

  - 否则 `dfs(i,j)=dfs(i,j−1) + 1`，说明 `dfs(i−1,j) > dfs(i,j−1)`，答案应为 `dfs(i,j−1)` 对应的最短公共超序列加上 `t[j]`

- 如果 `s[i] == t[j]`
  - 那么答案应为 `dfs(i-1,j−1)` 对应的最短公共超序列加上 `s[i]`

因此可以写一个类似 dfs 的递归函数 `makeString(i, j)`，通过比较 `dfs(i,j)` 与 `dfs(i−1,j)`，递归构造出具体的答案 (也可以直接比较 `dfs(i - 1, j)` 和 `dfs(i, j - 1)`)

```c++
class Solution {
public:
    string shortestCommonSupersequence(string str1, string str2) {
        int n = str1.size(), m = str2.size();

        vector<vector<int>> cache(n, vector<int>(m, -1));
        function<int(int, int)> dfs = [&](int i, int j) -> int {
            if (i < 0) return j + 1;
            if (j < 0) return i + 1;
            if (cache[i][j] != -1) return cache[i][j];
            if (str1[i] == str2[j]) return cache[i][j] = dfs(i - 1, j - 1) + 1;
            return cache[i][j] = min(dfs(i - 1, j), dfs(i, j - 1)) + 1;
        };

        function<string(int, int)> makeString = [&](int i, int j) -> string {
            if (i < 0) return str2.substr(0, j + 1);
            if (j < 0) return str1.substr(0, i + 1);
            if (str1[i] == str2[j]) {
                return makeString(i - 1, j - 1) + str1[i];
            }
            if (dfs(i - 1, j) < dfs(i, j - 1)) {
                return makeString(i - 1, j) + str1[i];
            } else {
                return makeString(i, j - 1) + str2[j];
            }
        };
        return makeString(n - 1, m - 1);
    }
};
```

- 时间复杂度：O(nm)。由于每个状态只会计算一次，因此动态规划的时间复杂度 = 状态个数 × 单个状态的计算时间。本题的状态个数为 O(nm)，单个状态的计算时间为 O(1)，因此记忆化搜索的时间复杂度为 O(nm)。构造答案的部分，递归 O(n+m) 次（注意 makeString 的递归树仅仅是一条链），每次拼接字符串，不同语言的实现不同，如果直接加在字符串末尾就是 O(1)，如果生成新的字符串就是 O(n+m)，所以构造答案这部分需要 O(n+m) 或 O((n+m)^2)) 的时间，这会在下面的代码中进一步优化成 O(n+m)

- 空间复杂度：O(nm)。一共有 O(nm) 个状态，需要 O(nm) 的空间实现记忆化

递推
```c++
class Solution {
public:
    string shortestCommonSupersequence(string str1, string str2) {
        int n = str1.size(), m = str2.size();
        // 计算最短公共超序列
        vector<vector<int>> f(n + 1, vector<int>(m + 1, 0));
        for (int i = 0; i < n; ++i) f[i + 1][0] = i + 1;
        for (int j = 0; j < m; ++j) f[0][j + 1] = j + 1;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < m; ++j) {
                if (str1[i] == str2[j]) f[i + 1][j + 1] = f[i][j] + 1;
                else f[i + 1][j + 1] = min(f[i][j + 1], f[i + 1][j]) + 1;
            }
        }
        // 构造答案
        string res = "";
        int i = n - 1, j = m - 1;
        while (i >= 0 && j >= 0) {
            if (str1[i] == str2[j]) {
                res += str1[i];
                --i, --j;
            } else if (f[i][j + 1] < f[i + 1][j]) {
                res += str1[i--];
            } else {
                res += str2[j--];
            }
        }
        reverse(res.begin(), res.end());
        // 可能最后并不是所有字符都用上了
        return str1.substr(0, i + 1) + str2.substr(0, j + 1) + res;
    }
};
```
- 时间复杂度：O(nm)

- 空间复杂度：O(nm)


> Go

```go
func shortestCommonSupersequence(str1 string, str2 string) string {
    n, m := len(str1), len(str2)
    f := make([][]int, n + 1)
    for i := range f {
        f[i] = make([]int, m + 1)
    }
    for i := range str1 {
        f[i + 1][0] = i + 1
    }
    for j := range str2 {
        f[0][j + 1] = j + 1
    }
    for i, c1 := range str1 {
        for j, c2 := range str2 {
            if c1 == c2 {
                f[i + 1][j + 1] = f[i][j] + 1
            } else {
                f[i + 1][j + 1] = min(f[i + 1][j], f[i][j + 1]) + 1
            }
        }
    }
    res := []byte{}
    i, j := n - 1, m - 1
    for i >= 0 && j >= 0 {
        if str1[i] == str2[j] {
            res = append(res, str1[i])
            i--
            j--
        } else if f[i][j + 1] < f[i + 1][j] {
            res = append(res, str1[i])
            i--
        } else {
            res = append(res, str2[j])
            j--            
        }
    }
    for i, n := 0, len(res); i < n / 2; i++ {
        res[i], res[n - 1 - i] = res[n - 1 - i], res[i]
    }
    return str1[:i + 1] + str2[:j + 1] + string(res) 
}

func min(a, b int) int { if b < a { return b }; return a }
```

## :lollipop: [72. 编辑距离](https://leetcode.cn/problems/edit-distance/description/)

- :cherry_blossom: 思路

其实思路不难, dfs 考虑哪个操作即可, 并且可以仅考虑一个字符串转化为另一个字符串所需的最少次数。因为 text1 插入等价于 text2 删除, text1 删除等价于 text2 插入, 修改操作改哪个都一样

dfs 和 DP 很快就写出来了, 但改一维 DP 还是没改出来 :sob:, 一直错 (当前状态也是跟左, 左上, 上相关), 直到用一个变量记录左上, 但还是忘了点东西

- **:beers: 代码**

> c++

```c++
// 常规 DP 以及记忆化
class Solution {
public:
    int minDistance(string word1, string word2) {
        int n = word1.size(), m = word2.size();
        vector<vector<int>> f(n + 1, vector<int>(m + 1, 0));
        for (int i = 0; i < n; ++i) f[i + 1][0] = i + 1;
        for (int j = 0; j < m; ++j) f[0][j + 1] = j + 1;
        f[0][0] = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < m; ++j) {
                if (word1[i] == word2[j]) f[i + 1][j + 1] = f[i][j];
                else f[i + 1][j + 1] = min({f[i + 1][j], f[i][j + 1], f[i][j]}) + 1;
            }
        }
        return f[n][m];

        // function<int(int, int)> dfs = [&](int i, int j) -> int {
        //     if (j < 0) return i + 1;
        //     if (i < 0) return j + 1;
        //     if (word1[i] == word2[j]) return dfs(i - 1, j - 1);
        //     else return min({dfs(i - 1, j), dfs(i, j - 1), dfs(i - 1, j - 1)}) + 1;
        // };
        // return dfs(n - 1, m - 1);
    }
};

// DP 空间优化
class Solution {
public:
  // 二维 DP 是初始化了第一行和第一列的, 所以改一维 DP 的时候每进入一行应该修改 f[0]
    int minDistance(string word1, string word2) {
        int n = word1.size(), m = word2.size();
        vector<int> f(m + 1, 0);
        for (int j = 0; j < m; ++j) f[j + 1] = j + 1;
        for (int i = 0; i < n; ++i) {
            int pre = i;
            // 自己忘了修改 f[0] 的值了, 一直错
            f[0] = i + 1;
            for (int j = 0; j < m; ++j) {
                int tmp = f[j + 1];
                if (word1[i] == word2[j]) f[j + 1] = pre;
                else f[j + 1] = min({f[j], f[j + 1], pre}) + 1;
                pre = tmp;
            }
        }
        return f[m];
    }
};
```

> Go

```go
// DP 空间优化
func minDistance(word1 string, word2 string) int {
    m := len(word2)
    f := make([]int, m + 1)
    for j := range word2 {
        f[j + 1] = j + 1
    }
    for i, c1 := range word1 {
        pre := i
        f[0] = i + 1;
        for j, c2 := range word2 {
            tmp := f[j + 1]
            if c1 == c2 {
                f[j + 1] = pre
            } else {
                f[j + 1] = min(f[j + 1], min(f[j], pre)) + 1
            }
            pre = tmp
        }
    }
    return f[m]
}

func min(a, b int) int {if a < b {return a}; return b}
```

## :lollipop: [834. 树中距离之和](https://leetcode.cn/problems/sum-of-distances-in-tree/description/)

- :cherry_blossom: 思路

自己只会暴力, 计算以每个节点为根的情况下的距离之和, 计算一次的时间复杂度为 O(n), 总时间复杂度为 O(n ^ 2), 超时

**本题是典型的换根 DP 题**

灵神的有张图总结的很好

![换根DP](https://pic.leetcode.cn/1689398667-omjvbD-lc834.png)


子树的大小为 1 + 所有子树的大小

- 在 DFS 中，如何保证每个节点只递归访问一次？

通用做法是用一个 vis 数组标记访问过的点，如果某个点之前访问过，就不再递归访问。但对于树来说，一直向下递归，并不会遇到之前访问过的点，所以不需要 vis 数组。本题是无向树，除了根节点以外，其余每个点的邻居都包含其父节点，所以要避免访问父节点。我们可以定义 dfs(x,fa) 表示递归到节点 x 且 x 的父节点为 fa。只要 x 的邻居 y != fa 就可以 dfs(y,x) 向下递归了

- 这种算法的本质是什么？

以图中的这棵树为例，从以 0 为根换到以 2 为根时，原来 2 的子节点还是 2 的子节点，原来 1 的子节点还是 1 的子节点，唯一改变的是 0 和 2 的父子关系。由此可见，一对节点的距离的「变化量」应该是很小的，那么找出「变化量」的规律，就可以基于 res[0] 算出 res[2] 了。这种算法叫做换根 DP

- **:beers: 代码**

> c++

```c++
class Solution {
public:
    vector<int> sumOfDistancesInTree(int n, vector<vector<int>>& edges) {
        vector<vector<int>> g(n);
        for (auto &e : edges) {
            int x = e[0], y = e[1];
            g[x].emplace_back(y);
            g[y].emplace_back(x);
        }
        vector<int> res(n, 0), size(n, 1);
        // 计算以 0 为根时的路径和并统计每颗子树的大小, 传入父节点避免重复计算
        function<void(int, int, int)> dfs = [&](int x, int fa, int depth) {
            res[0] += depth; 
            for (int y : g[x]) { // 遍历 x 的所有邻居
                if (y != fa) {
                    dfs(y, x, depth + 1);
                    size[x] += size[y]; // 统计每颗子树的大小
                }
            }
        };
        dfs(0, -1, 0); // 计算以 0 为根的结果
        // 计算其它节点的结果, 已经不需要深度信息了, 利用 x 的结果计算它的子树的结果
        // 不妨设 x 的一个子节点为 y, 那么根从 x 换到 y 后, y 到所有 y 的子节点的距离 - 1, 其余节点距离 + 1
        //  res[y] = res[x] - 1 * (size[y]) + (n - size[y]) = res[x] + n - 2 * size[y]
        function<void(int, int)> dfs2 = [&](int x, int fa) {
            for (int y : g[x]) {
                if (y != fa) {
                    res[y] = res[x] + n - 2 * size[y];
                    dfs2(y, x);
                }
            }
        };
        dfs2(0, -1);
        return res;
    }
};
```
时间复杂度：O(n)。DFS 两次，每次 DFS 会递归访问每个节点恰好一次，所以时间复杂度为 O(n)

空间复杂度：O(n)


> Go

```go
func sumOfDistancesInTree(n int, edges [][]int) []int {
    g := make([][]int, n)
    for _, e := range edges {
        x , y := e[0], e[1]
        g[x] = append(g[x], y)
        g[y] = append(g[y], x)
    }
    res := make([]int, n)
    size := make([]int, n)
    var dfs func(int, int, int)
    dfs = func(x, fa, depth int) {
        res[0] += depth
        size[x] = 1
        for _, y := range g[x] {
            if y != fa {
                dfs(y, x, depth + 1)
                size[x] += size[y]
            }
        }
    };
    dfs(0, -1, 0)

    var dfs2 func(int, int)
    dfs2 = func(x, fa int) {
        for _, y := range g[x] {
            if y != fa {
                res[y] = res[x] + n - 2 * size[y]
                dfs2(y, x)
            }
        }
    }
    dfs2(0, -1)
    return res
}
```

## :lollipop: [310. 最小高度树](https://leetcode.cn/problems/minimum-height-trees/description/)

确定以哪个节点为根时树高最小, 可能有多个答案

![](https://assets.leetcode.com/uploads/2020/09/01/e1.jpg)
- :cherry_blossom: 思路


**方法一、换根 DP, 思考已知某个节点的答案后如何推出它的子树的答案**

![](https://pic.leetcode-cn.com/1649213331-YrViTz-%E6%9C%80%E5%B0%8F%E9%AB%98%E5%BA%A6%E6%A0%91.jpg)

若已知以 r 为根时的答案, 那么根从 r 换到 u 时, 除了 r 和 u 节点外, 其余节点的高度不变!!

并且 u 的高度为下面两者的最大值
 - 以 r 为根时子树 u 的高度
 - 以 r 为根时, r 排除 u 这颗子树的高度 + 1

为了快速计算高度, 再第一遍遍历根节点 0 时可以统计出所有节点的高度 height, 这样在换根后更改 height[r] 和 height[u] 即可

但是每次重新遍历 r 的所有非 u 子树计算最大值太慢了, 会超时 (最后一个案例过不了), 有个优化小技巧

直接计算以 r 为根时所有子树高度的最大值 first 以及次大值 second, 那么排除 u 后子树的最大值必定为 first + 1 或 second + 1; 

当 height[u] == first 时, height'[r] = second + 1, 否则 height'[r] = first + 1; 那么 res[u] = max(height[u], height'[r] + 1), 随后更新 height'[u] = res[u], 继续向下走即可

**方法二、类似拓扑排序**

从最外侧的节点开始删, 每次删除一层节点, 最后一次删除的节点集合就是答案

统计每个节点的孩子数目, 因为是无向的, 所以从子节点数目为 1 的节点开始删

- **:beers: 代码**

> c++

```c++
// 方法一、换根 DP
class Solution {
public:
    vector<int> findMinHeightTrees(int n, vector<vector<int>>& edges) {
        vector<vector<int>> g(n);
        for (auto &e : edges) {
            int x = e[0], y = e[1];
            g[x].emplace_back(y);
            g[y].emplace_back(x);
        }
        int mn = n; // 保存最小高度
        vector<int> height(n, 1); // 以某个节点为根时各个节点的高度
        vector<int> res(n, 1); // 以每个节点为根的高度
        // 计算以 0 为根时, 以各个节点为根的子树高
        function<int(int, int)> dfs = [&](int x, int fa) -> int {
            int h = 1;
            for (int y : g[x])
                if (y != fa) 
                    h = max(dfs(y, x) + 1, h);
            height[x] = h;
            return h;
        };
        dfs(0, -1);
        mn = res[0] = height[0];
        // 已知父节点 x 的结果, 推它的子节点的结果, 不妨设 x 的一个子节点为 y
        // 那么根从 x 换到 y 后, 对于除 x 和 y 的所有节点而言, 它们子树高度不会因为根是 x 还是 y 而改变 (height 不变)
        // y 的结果要么是 height[y] (以 x 为根时, y 的子树的高度), 要么是 x 排除 y 这颗子树后的高度 + 1
        function<void(int, int)> dfs2 = [&](int x, int fa) {
            for (int y : g[x]) {
                if (y != fa) {
                    int mx = 0; // 保存 x 排除 y 这颗子树后的最大高度, 每次再遍历一遍超时
                    for (int son : g[x]) {
                        if (son != y)
                            mx = max(mx, height[son] + 1);
                    }
                    // 继续往下递归的时候 height 应表示以 y 为根时所有节点的高度了
                    // 只需要改变两个节点, height[x] 即为 mx, height[y] 即为算出来的答案, 回溯处理
                    int yuan_x = height[x], yuan_y = height[y];
                    height[x] = mx;
                    res[y] = max(height[y], height[x] + 1);
                    height[y] = res[y];
                    mn = min(mn, res[y]);
                    dfs2(y, x);
                    height[x] = yuan_x, height[y] = yuan_y;
                }
            }
        };
        dfs2(0, -1);
        vector<int> ans;
        for (int i = 0; i < n; ++i) {
            if (res[i] == mn) ans.emplace_back(i);
        }
        return ans;
    }
};

// 最大值次大值优化
class Solution {
public:
    vector<int> findMinHeightTrees(int n, vector<vector<int>>& edges) {
        vector<vector<int>> g(n);
        for (auto &e : edges) {
            int x = e[0], y = e[1];
            g[x].emplace_back(y);
            g[y].emplace_back(x);
        }
        int mn = n; // 保存最小高度
        vector<int> height(n, 1); // 以某个节点为根时各个节点的高度
        vector<int> res(n, 1); // 以每个节点为根的高度
        // 计算以 0 为根时, 以各个节点为根的子树高
        function<int(int, int)> dfs = [&](int x, int fa) -> int {
            int h = 1;
            for (int y : g[x])
                if (y != fa) 
                    h = max(dfs(y, x) + 1, h);
            height[x] = h;
            return h;
        };
        dfs(0, -1);
        mn = res[0] = height[0];
        function<void(int, int)> dfs2 = [&](int x, int fa) {
            int first = 0, second = 0; // 保存 x 的最大以及次大高度
            for (int y : g[x]) {
                if (height[y] > first) {
                    second = first;
                    first = height[y];
                } else if (height[y] > second) {
                    second = height[y];
                }
            }
            for (int y : g[x]) {
                if (y != fa) {
                    // 继续往下递归的时候 height 应表示以 y 为根时所有节点的高度了
                    // 利用最大值以及次大值的技巧, 因为 height[x] 不是 first + 1 就是 second + 1 (根据 height[y] == first)
                    int yuan_x = height[x], yuan_y = height[y];
                    height[x] = (height[y] == first ? second : first) + 1;
                    res[y] = max(height[y], height[x] + 1);
                    height[y] = res[y];
                    mn = min(mn, res[y]);
                    dfs2(y, x);
                    height[x] = yuan_x, height[y] = yuan_y;
                }
            }
        };
        dfs2(0, -1);
        vector<int> ans;
        for (int i = 0; i < n; ++i) {
            if (res[i] == mn) ans.emplace_back(i);
        }
        return ans;
    }
};
// 其实也不用回溯, 因为 first 和 second 是之前算好的, 每次改 height[x] 必定不会错
// 并且只会向下走, 所以访问了 y 以后 y 的父节点不可能受 y 影响, 只有子节点会受影响, 因此即使 height[y] 并不是当前 根节点下的 height, 不会影响它向其他子树的递推计算
class Solution {
public:
    vector<int> findMinHeightTrees(int n, vector<vector<int>>& edges) {
        vector<vector<int>> g(n);
        for (auto &e : edges) {
            int x = e[0], y = e[1];
            g[x].emplace_back(y);
            g[y].emplace_back(x);
        }
        int mn = n; 
        vector<int> height(n, 1); 
        vector<int> res(n, 1); 
        function<int(int, int)> dfs = [&](int x, int fa) -> int {
            int h = 1;
            for (int y : g[x])
                if (y != fa) 
                    h = max(dfs(y, x) + 1, h);
            height[x] = h;
            return h;
        };
        dfs(0, -1);
        mn = res[0] = height[0];
        function<void(int, int)> dfs2 = [&](int x, int fa) {
            int first = 0, second = 0; // 保存 x 的最大以及次大高度
            for (int y : g[x]) {
                if (height[y] > first) {
                    second = first;
                    first = height[y];
                } else if (height[y] > second) {
                    second = height[y];
                }
            }
            for (int y : g[x]) {
                if (y != fa) {
                    height[x] = (height[y] == first ? second : first) + 1;
                    res[y] = max(height[y], height[x] + 1);      
                    mn = min(mn, res[y]);
                    dfs2(y, x);
                }
            }
        };
        dfs2(0, -1);
        vector<int> ans;
        for (int i = 0; i < n; ++i) {
            if (res[i] == mn) ans.emplace_back(i);
        }
        return ans;
    }
};

// 方法二、类似拓扑排序
class Solution {
public:
    vector<int> findMinHeightTrees(int n, vector<vector<int>>& edges) {
        if (edges.size() == 0) {
            vector<int> res(n);
            iota(res.begin(), res.end(), 0);
            return res;
        }
        vector<vector<int>> g(n);
        vector<int> degree(n, 0);
        for (auto &e : edges) {
            int x = e[0], y = e[1];
            g[x].emplace_back(y);
            g[y].emplace_back(x);
            ++degree[x], ++degree[y];
        }
        vector<int> res;
        queue<int> q;
        for (int i = 0; i < n; ++i) 
            if (degree[i] == 1) q.emplace(i);

        while (!q.empty()) {
            vector<int>().swap(res);
            int size = q.size();
            while (size--) {
                auto node = q.front();
                res.emplace_back(node);
                q.pop();
                --degree[node];
                for (int next : g[node]) 
                    if (--degree[next] == 1) q.emplace(next);
            }
        }
        return res;
    }
};
```

> Go

```go
// 方法一、换根 DP
func findMinHeightTrees(n int, edges [][]int) []int {
    g := make([][]int, n)
    for _, edge := range edges {
        x, y := edge[0], edge[1]
        g[x] = append(g[x], y)
        g[y] = append(g[y], x)
    }
    height := make([]int, n)
    res := make([]int, n)
    var dfs func(int, int) int
    dfs = func(x, fa int) int {
        h := 0
        for _, y := range g[x] {
            if y != fa {
                h = max(h, dfs(y, x))
            }
        }
        height[x] = h + 1
        return height[x]
    }
    dfs(0, -1)
    res[0] = height[0]
    mn := res[0]
    var dfs2 func(int, int)
    dfs2 = func(x, fa int) {
        first, second := 0, 0
        for _, y := range g[x] {
            if height[y] > first {
                second = first
                first = height[y]
            } else if height[y] > second  {
                second = height[y]
            }
        }
        for _, y := range g[x] {
            if y != fa {
                if height[y] == first {
                    height[x] = second + 1
                } else {
                    height[x] = first + 1
                }
                res[y] = max(height[y], height[x] + 1)
                mn = min(mn, res[y])
                dfs2(y, x)
            }
        }
    }
    dfs2(0, -1)
    ans := []int{}
    for i, val := range res {
        if val == mn {
            ans = append(ans, i)
        }
    }
    return ans
}

func min(a, b int) int {if a < b {return a}; return b}
func max(a, b int) int {if a < b {return b}; return a}

// 方法二、类似拓扑排序
func findMinHeightTrees(n int, edges [][]int) []int {
    if len(edges) == 0 {
        res := make([]int, n)
        for i := range res {
            res[i] = i
        }
        return res
    }
    g := make([][]int, n)
    degree := make([]int, n)
    for _, edge := range edges {
        x, y := edge[0], edge[1]
        g[x] = append(g[x], y)
        g[y] = append(g[y], x)
        degree[x]++
        degree[y]++
    }
    q := []int{}
    for i, val := range degree {
        if val == 1 {
            q = append(q, i)
        }
    }
    res := []int{}
    for len(q) > 0 {
        res = []int{}
        size := len(q)
        for size > 0 {
            node := q[0]
            q = q[1:]
            res = append(res, node)
            degree[node]--
            for _, next := range g[node] {
                degree[next]--
                if degree[next] == 1 {
                    q = append(q, next)
                }
            }
            size--
        }
    }
    return res
}
```

## :lollipop: [1000. 合并石头的最低成本](https://leetcode.cn/problems/minimum-cost-to-merge-stones/description/)

- :cherry_blossom: 思路

自己不会

区间 DP

注意虽然涉及到了很多次的合并，但是并不需要修改数组，因为合并的代价就为数组的区间和，可以利用前缀和预处理

区间 DP 的推导思路 (灵神的，很清晰)

![](https://pic.leetcode.cn/1680534488-qZHfMY-1000-3d-cut.png)

- 什么时候输出 −1 呢？

从 n 堆变成 1 堆，需要减少 n−1 堆。而每次合并都会减少 k−1 堆，所以 n−1 必须是 k−1 的倍数

- 为什么只考虑分出 1 堆和 p−1 堆，而不考虑分出 x 堆和 p−x 堆？

答：无需计算，因为 p−1 堆继续递归又可以分出 1 堆和 p−2 堆，和之前分出的 1 堆组合，就已经能表达出「分出 2 堆和 p−2 堆」的情况了。其他同理。所以只需要考虑分出 1 堆和 p−1 堆


- **:beers: 代码**

> c++

```c++
// 记忆化
class Solution {
public:
    int mergeStones(vector<int>& stones, int k) {
        int n = stones.size();
        if ((n - 1) % (k - 1)) return -1;
        vector<int> sum(n + 1, 0); // 保存前缀和
        for (int i = 0; i < n; ++i)
            sum[i + 1] = sum[i] + stones[i];
        // 返回把 i ~ j 合成 p 堆的最小成本
        // 那么就需要枚举把 i ~ m 合成一堆 和 把 m + 1 ~ j 合成 p - 1 堆的最小成本
        // 为了真的可以合成一堆, m 应每次加 k - 1
        vector<vector<vector<int>>> cache(n, vector<vector<int>>(n, vector<int>(k + 1, -1)));
        function<int(int, int, int)> dfs = [&](int i, int j, int p) -> int {
            int &res = cache[i][j][p];
            if (res != -1) return res;
            if (p == 1) { // 合并成 1 堆
                // 若 i == j, 返回 0; 否则先将所有石头合成 k 堆再进行一次合并即可
                return res = i == j ? 0 : dfs(i, j, k) + sum[j + 1] - sum[i];
            }
            res = INT_MAX;
            // 枚举哪些石头堆合并成第一堆, 其余石头合并为 p - 1 堆
            for (int m = i; m < j; m += k - 1) {
                res = min(res, dfs(i, m, 1) + dfs(m + 1, j, p - 1));
            }
            return res;
        };
        return dfs(0, n - 1, 1);
    }
};
```

时间复杂度：O(n^3)，其中 n 为 stones 的长度。动态规划的时间复杂度 = 状态个数 × 单个状态的计算时间。这里状态个数为 O(n^2k)，单个状态的计算时间为 O(n/k)，因此时间复杂度为 O(n^3)

空间复杂度：O(n^2*k)

![](https://pic.leetcode.cn/1680534839-WSibYe-1000-2d.png)

```c++
class Solution {
public:
    int mergeStones(vector<int>& stones, int k) {
        int n = stones.size();
        if ((n - 1) % (k - 1)) return -1;

        vector<int> sum(n + 1, 0); // 保存前缀和
        for (int i = 0; i < n; ++i)
            sum[i + 1] = sum[i] + stones[i];
        // 计算将 [i, j] 合并成一堆石头的最小成本
        vector<vector<int>> cache(n, vector<int>(n, -1));
        function<int(int, int)> dfs = [&](int i, int j) -> int {
            if (i == j) return 0; // 只有一堆，不用合并
            int &res = cache[i][j];
            if (res != -1) return res;
            res = INT_MAX;
            // 不断的将子数组分成更小的片段, 递归的计算左侧子数组和右侧子数组合并成一堆的最小成本
            for (int m = i; m < j; m += k - 1) {
                res = min(res, dfs(i, m) + dfs(m + 1, j));
            }
            // 若当前子数组可以合并为一堆的话，增加成本直接返回即可
            if ((j - i) % (k - 1) == 0) // 可以合并成一堆
                res += sum[j + 1] - sum[i];
            return res;
        };
        return dfs(0, n - 1);
    }
};

// 递推
class Solution {
public:
    int mergeStones(vector<int>& stones, int k) {
        int n = stones.size();
        if ((n - 1) % (k - 1)) return -1;

        vector<int> sum(n + 1, 0); // 保存前缀和
        for (int i = 0; i < n; ++i)
            sum[i + 1] = sum[i] + stones[i];
        
        vector<vector<int>> f(n, vector<int>(n, 0));
        for (int i = n - 1; i >= 0; --i) {
            f[i][i] = 0;
            for (int j = i + 1; j < n; ++j) {
                f[i][j] = INT_MAX;
                for (int m = i; m < j; m += k - 1) {
                    f[i][j] = min(f[i][j], f[i][m] + f[m + 1][j]);
                }
                if ((j - i) % (k - 1) == 0) {
                    f[i][j] += sum[j + 1] - sum[i];
                }
            }
        }
        return f[0][n - 1];
    }
};
```

时间复杂度：O(n^3\k)，其中 n 为 stones 的长度。动态规划的时间复杂度 = 状态个数 × 单个状态的计算时间。这里状态个数为 O(n^2)，单个状态的计算时间为 O(nk)，因此时间复杂度为 O(n^3\k)

空间复杂度：O(n^2)


> Go

```go

```


## :lollipop: [2376. 统计特殊整数](https://leetcode.cn/problems/count-special-integers/description/)

- :cherry_blossom: 思路

自己不会

**数位 DP (灵神的板子)**

将 n 转化为 字符串 s, 定义 `f(i, mask, isLimit, isNum)` 表示构造第 i 位及其之后数位的合法方案数，其余参数的含义为:

- `mask`: 表示前面选过的数字集合，换句话说，第 i 位要选的数字不能在 `mask` 中

- `isLimit` 表示当前是否受到了 n 的约束（注意要构造的数字不能超过 n）。若为真，则第 i 位填入的数字至多为 `s[i]`，否则可以是 9。如果在受到约束的情况下填了 `s[i]`，那么后续填入的数字仍会受到 n 的约束。例如 n=123，那么 i=0 填的是 1 的话，i=1 的这一位至多填 2

- `isNum` 表示 i 前面的数位是否填了数字。若为假，则当前位可以跳过（不填数字），或者要填入的数字至少为 1；若为真，则要填入的数字可以从 0 开始。例如 n=123，在 i=0 时跳过的话，相当于后面要构造的是一个 9 以内的数字了，如果 i=1 不跳过，那么相当于构造一个 10 到 99 的两位数，如果 i=1 跳过，相当于构造的是一个 9 以内的数字

**实现细节**

递归入口：`f(0, 0, true, false)`，表示：

- 从 `s[0]` 开始枚举；
- 一开始集合中没有数字；
- 一开始要受到 n 的约束（否则就可以随意填了，这肯定不行）；
- 一开始没有填数字。

递归中：

- 如果 `isNum` 为假，说明前面没有填数字，那么当前也可以不填数字。一旦从这里递归下去， `isLimit` 就可以置为 `false` 了，这是因为 `s[0]` 必然是大于 0 的，后面就不受到 n 的约束了。或者说，最高位不填数字，后面无论怎么填都比 n 小
- 如果 `isNum` 为真，那么当前必须填一个数字。枚举填入的数字，根据 `isNum` 和 `isLimit` 来决定填入数字的范围

递归终点：当 i 等于 s 长度时，如果 `isNum` 为真，则表示得到了一个合法数字（因为不合法的不会继续递归下去），返回 1，否则返回 0

**问：`isNum` 这个参数可以去掉吗？**

**答**：对于本题是可以的。由于 `mask` 中记录了数字，可以通过判断 `mask` 是否为 0 来判断前面是否填了数字，所以 `isNum` 可以省略。

下面的代码保留了 `isNum`, 主要是为了方便大家掌握这个模板。因为有些题目不需要 `mask`, 但需要 `isNum`

**问：记忆化四个状态有点麻烦，能不能只记忆化 (i, mask) 这两个状态？**

**答**：是可以的。比如 n=234，第一位填 2，第二位填 3，后面无论怎么递归，都不会再次递归到第一位填 2，第二位填 3 的情况，所以不需要记录。又比如，第一位不填，第二位也不填，后面无论怎么递归也不会再次递归到这种情况，所以也不需要记录。

根据这个例子，我们可以只记录不受到 isLimit 或 isNum 约束时的状态 (i,mask)。比如 n=234，第一位（最高位）填的 1，那么继续递归，后面就可以随便填，所以状态 (1,2) 就表示前面填了一个 1（对应的 mask=2），从第二位往后随便填的方案数

**问：能不能只记忆化 i？**

**答**：这是不行的。想一想，我们为什么要用记忆化？如果递归到同一个状态时，计算出的结果是一样的，那么第二次递归到同一个状态，就可以直接返回第一次计算的结果了。通过保存第一次计算的结果，来优化时间复杂度。

由于前面选的数字会影响后面选的数字，两次递归到相同的 i，如果前面选的数字不一样，计算出的结果就可能是不一样的。如果只记忆化 i，就可能会算出错误的结果。

也可以这样理解：记忆化搜索要求递归函数无副作用（除了修改 `cache` 数组），从而保证递归到同一个状态时，计算出的结果是一样的


**总结**

- 记忆化仅记忆不受限制且为数字的结果

- 遍历的数字的上界由 isLimit 决定
  
  - isLimit 为 true 的话上界为当前数字
    
  - isLimit 为 false 的话上界任取，可以为 9 

- 遍历的数字的下界由 isNum 决定

  - isNum 为 true, 下界为 0

  -  isNum 为 false, 可以直接跳过计算结果 (跳过的话 isLimit 就为 false), 再计算不跳过的结果, 此时下界为 1 (跳过相当于为 0 )

- 往下递归的时候参数的改变

- mask 加上当前选的即可, 即 mask = mask | (1 << d)

- isLimit 跟当前的 isLimit 和选的数相关, isLimit = isLimit && d == up (当且仅当目前受限并且选的仍为最大值时仍受限)

-  isNum 只要选了数字就为 true, 只有为 false 时并且直接跳过时才接着为 false

根据题意决定要不要 isLimit 以及 isNum 即可, 板子：

```c++
function<int(int, int, bool, bool)> dfs = [&](int i, int mask, bool isLimit, bool isNum) -> int {
   if (i == m) return isNum; // 到达末尾并且为数字, 返回 1
   // 去掉另外两个参数的影响
   if (!isLimit && isNum && cache[i][mask] != -1) return cache[i][mask];
   int res = 0;
   if (!isNum) { // 当前不是数字的话可以跳过, 跳过后就不受 n 的约束了
       res = dfs(i + 1, mask, false, false);
   }
   int up = isLimit ? s[i] - '0' : 9; // 确定数字上界
   // 下界跟 isNum 相关, 若是数字从 0 开始，否则从 1 开始
   for (int d = 1 - isNum; d <= up; ++d) {
       if ((mask >> d & 1) == 0) { // 当前数字没选过
           res += dfs(i + 1, mask | (1 << d), isLimit && d == up, true);
       }
   }
   if (!isLimit && isNum) { // 只记忆化不受制约并且是数字的结果
       cache[i][mask] = res;
   }
   return res;
};
```

- **:beers: 代码**

> c++

```c++
class Solution {
public:
    int countSpecialNumbers(int n) {
        auto s = to_string(n);
        int m = s.size(), cache[m][1 << 10]; // 表示1 到 9 是否选过
        memset(cache, -1, sizeof(cache));
        function<int(int, int, bool, bool)> dfs = [&](int i, int mask, bool isLimit, bool isNum) -> int {
            if (i == m) return isNum;
            // 去掉另外两个参数的影响
            if (!isLimit && isNum && cache[i][mask] != -1) return cache[i][mask];
            int res = 0;
            if (!isNum) { // 当前不是数字的话可以跳过, 跳过后就不受 n 的约束了
                res = dfs(i + 1, mask, false, false);
            }
            int up = isLimit ? s[i] - '0' : 9; // 确定数字上界
            // 下界跟 isNum 相关, 若是数字从 0 开始，否则从 1 开始
            for (int d = 1 - isNum; d <= up; ++d) {
                if ((mask >> d & 1) == 0) { // 当前数字没选过
                    res += dfs(i + 1, mask | (1 << d), isLimit && d == up, true);
                }
            }
            if (!isLimit && isNum) { // 只记忆化不受制约并且是数字的结果
                cache[i][mask] = res;
            }
            return res;
        };
        return dfs(0, 0, true, false);
    }
};
```

*   时间复杂度：`O(m*D*2^D)`，其中 m 为 s 的长度，即 O(log⁡n)；D=10。由于每个状态只会计算一次，因此动态规划的时间复杂度 = 状态个数 × 单个状态的计算时间。本题状态个数为 O(m*2^D)，单个状态的计算时间为 O(D)，因此时间复杂度为 `O(m*D*2^D)`
  
*   空间复杂度：O(m*2^D)

> Go

```go
func countSpecialNumbers(n int) int {
    s := strconv.Itoa(n)
    m := len(s)
    cache := make([][1 << 10]int, m)
    for i := range cache {
        for j := range cache[i] {
            cache[i][j] = -1
        }
    }
    var dfs func(int, int, bool, bool) int
    dfs = func(i, mask int, isLimit, isNum bool) int {
        if i == m {
            if isNum {
                return 1
            }
            return 0
        }
        if !isLimit && isNum && cache[i][mask] != -1 {
            return cache[i][mask]
        }
        res := 0
        up, low := 9, 0
        if !isNum {
            res = dfs(i + 1, mask, false, false)
            low = 1
        }
        if isLimit {
            up = int(s[i] - '0')
        }
        for ; low <= up; low++ {
            if mask >> low & 1 == 0 {
                res += dfs(i + 1, mask | (1 << low), isLimit && low == up, true)
            }
        }
        if !isLimit && isNum {
            cache[i][mask] = res
        }
        return res
    }
    return dfs(0, 0, true, false)
}
```

## :lollipop: [233. 数字 1 的个数](https://leetcode.cn/problems/number-of-digit-one/description/)

同[面试题 17.06. 2出现的次数](https://leetcode.cn/problems/number-of-2s-in-range-lcci/description/)

- :cherry_blossom: 思路

自己写的数位 DP 的第二题，还是没写出来

定义 `dfs(i, cnt, isLimit, isNum)` 表示构造从左往右第 i 位, 已经出现了 `cnt` 个 1，在这种情况下，继续构造最终会得到的 1 的个数（你可以直接从回溯的角度理解这个过程，只不过是多了个记忆化）

对于本题来说，由于前导零对答案无影响, `isNum` 可以省略

对于一个固定的 `(i,cnt1)`，这个状态受到 `isLimit` 的约束在整个递归过程中至多会出现一次，没必要记忆化

另外，如果只记忆化 `(i,cnt1)`，dp 数组的含义就变成在不受到约束时的合法方案数，所以要在 `!isLimit` 成立时才去记忆化


- **:beers: 代码**

> c++

```c++
class Solution {
public:
    int countDigitOne(int n) {
        string s = to_string(n);
        int m = s.size();
        // 记忆化 i 和 cnt 的原因为后面数字任选的情况下, 后面能选的 1 的数目是相等的
        // cache[i][num] 表示 i + 1 ~ 最后一位任选并且当前已经选了 num 个 1 的情况下能选的 1 的个数
        // 12XXX 能选的 1 的个数跟 13XXX, 21XXX, 31XXX, ... 是一样的
        vector<vector<int>> cache(m, vector<int>(m, -1));
        function<int(int, int, bool)> dfs = [&](int i, int cnt, bool isLimit) -> int {
            if (i == m) return cnt;
            if (!isLimit && cache[i][cnt] != -1) return cache[i][cnt];
            int up = isLimit ? s[i] - '0' : 9, res = 0;
            for (int d = 0; d <= up; ++d) {
                res += dfs(i + 1, cnt + (d == 1), isLimit && d == up);
            }
            if (!isLimit) {
                cache[i][cnt] = res;
            }
            return res;
        };
        return dfs(0, 0, true);
    }
};
```

> Go

```go
func countDigitOne(n int) int {
    s := strconv.Itoa(n)
    m := len(s)
    cache := make([][]int, m)
    for i := range cache {
        cache[i] = make([]int, m)
        for j := range cache[i] {
            cache[i][j] = -1
        }
    }
    var dfs func(int, int, bool) int
    dfs = func(i, cnt int, isLimit bool) int {
        if i == m {
            return cnt
        }
        if !isLimit && cache[i][cnt] != -1 {
            return cache[i][cnt]
        }
        up, res := 9, 0
        if isLimit {
            up = int(s[i] - '0')
        }
        for d := 0; d <= up; d++ {
            if d == 1 {
                res += dfs(i + 1, cnt + 1, isLimit && d == up)
            } else {
                res += dfs(i + 1, cnt, isLimit && d == up)
            }
        }
        if !isLimit {
            cache[i][cnt] = res
        }
        return res
    }
    return dfs(0, 0, true)
}
```

## :lollipop: [600. 不含连续1的非负整数](https://leetcode.cn/problems/non-negative-integers-without-consecutive-ones/description/)

- :cherry_blossom: 思路

数位 DP, 板子题，终于写出来了一次

不过还有可以优化的地方, 自己写的不够优雅

- **:beers: 代码**

> c++

```c++
// 自己写的，用位运算
class Solution {
public:
    int findIntegers(int n) {
        int m = 0;
        while (n >> m) ++m;
        int cache[m][2];
        memset(cache, -1, sizeof(cache));
        // 返回考虑第 i 位往后的数字并且前一个选了 last 的情况下的不含连续 1 的非负整数
        // 因为 10XXX 和 00XXX 的结果相同 (只要能走到第 m 位, 结果就加1)
        function<int(int, int, bool)> dfs = [&](int i, int last, bool isLimit) -> int {
            if (i == m) return 1;
            if (!isLimit && cache[i][last] != -1) return cache[i][last];
            int res = 0;
            // 不管上一个选的是什么，必定可以选 0
            // 原来数字的第 i 位为 n >> (m - 1 - i) & 1
            res += dfs(i + 1, 0, isLimit && 0 == ((n >> m - 1 - i) & 1));
            // 当前想选 1, 上一位必须为 0, 若当前无限制则可选 1, 若有限制得看原来是不是 1
            if (last == 0 && (!isLimit || (n >> m - 1 - i) & 1)) {
                res += dfs(i + 1, 1, isLimit && 1 == ((n >> m - 1 - i) & 1));
            }
            if (!isLimit) {
                cache[i][last] = res;
            }
            return res;
        };
        return dfs(0, 0, true);
    }
};

// 可以用 bitset<32>(n)转化为二进制，然后完全套板子
class Solution {
public:
    int findIntegers(int n) {
        string s = bitset<32>(n).to_string();
        int m = s.size();
        int cache[m][2];
        memset(cache, -1, sizeof(cache));
        // 返回考虑第 i 位往后的数字并且前一个选了 last 的情况下的不含连续 1 的非负整数
        // 因为 10XXX 和 00XXX 的结果相同 (只要能走到第 m 位, 结果就加1)
        function<int(int, int, bool)> dfs = [&](int i, int last, bool isLimit) -> int {
            if (i == m) return 1;
            if (!isLimit && cache[i][last] != -1) return cache[i][last];
            int up = isLimit ? s[i] - '0' : 1, res = 0;
            for (int d = 0; d <= up; ++d) {
                if (d == 1 && last == 1) continue;
                res += dfs(i + 1, d, isLimit && d == up);
            }
            if (!isLimit) {
                cache[i][last] = res;
            }
            return res;
        };
        return dfs(0, 0, true);
    }
};
```

> Go

```go
// Go 可以用 strconv.FormatInt(int64(n), 2) 转二进制字符串
func findIntegers(n int) int {
    s := strconv.FormatInt(int64(n), 2)
    m := len(s)
    cache := make([][2]int, m)
    for i := range cache {
        cache[i] = [2]int{-1, -1}
    }
    var dfs func(int, int, bool) int
    dfs = func(i, last int, isLimit bool) int {
        if i == m {
            return 1
        }
        if !isLimit && cache[i][last] != -1 {
            return cache[i][last]
        }
        up := 1
        if isLimit {
            up = int(s[i] & 1)
        }
        res := dfs(i + 1, 0, isLimit && up == 0) // 填 0
        if last == 0 && up == 1 { // 可以填 1
            res += dfs(i + 1, 1, isLimit)
        }
        if !isLimit {
            cache[i][last] = res
        }
        return res
    }
    return dfs(0, 0, true)
}
```

## :lollipop: [1012. 至少有 1 位重复的数字](https://leetcode.cn/problems/numbers-with-repeated-digits/description/)

- :cherry_blossom: 思路

自己写的比较繁琐，新增了一个参数表示当前是否选了重复数字，空间复杂度更大了 (大了一倍)，时间复杂度不变，都是总的数字总和

**正难则反** (这个思路需要好好掌握一下)

正着来不好写的话可以逆着来, 求小于等于整数 n 的无重复数字的数字数目, 那么答案就是 n - 无重复数字的数字数目了, 即[2376. 统计特殊整数](https://leetcode.cn/problems/count-special-integers/description/)

- **:beers: 代码**

> c++

```c++
// 自己的，新加了一个参数，过倒是也能过，56ms
class Solution {
public:
    int numDupDigitsAtMostN(int n) {
        string s = to_string(n);
        int m = s.size();
        int cache[m][1 << 10][2];
        memset(cache, -1, sizeof(cache));
        function<int(int, int, bool, bool, bool)> dfs = [&](int i, int mask, bool duplicate, bool isLimit, bool isNum) -> int {
            if (i == m) return duplicate;
            if (!isLimit && isNum && cache[i][mask][duplicate] != -1) return cache[i][mask][duplicate];
            int res = 0;
            if (!isNum) {
                res = dfs(i + 1, mask, false, false, false);
            }
            int up = isLimit ? s[i] - '0' : 9;
            for (int d = 1 - isNum; d <= up; ++d) {
                res += dfs(i + 1, mask | 1 << d, duplicate || ((mask >> d) & 1), isLimit && d == up, true);
            }
            if (!isLimit && isNum) {
                cache[i][mask][duplicate] = res;
            }
            return res;
        };
        return dfs(0, 0, false, true, false);
    }
};

// 逆向思路
class Solution {
public:
    int numDupDigitsAtMostN(int n) {
        string s = to_string(n);
        int m = s.size();
        int cache[m][1 << 10];
        memset(cache, -1, sizeof(cache));
        function<int(int, int, bool, bool)> dfs = [&](int i, int mask, bool isLimit, bool isNum) ->int {
            if (i == m) return isNum;
            if (!isLimit && isNum && cache[i][mask] != -1) return cache[i][mask];
            int res = 0;
            if (!isNum) {
                res = dfs(i + 1, mask, false, false);
            }
            int up = isLimit ? s[i] - '0' : 9;
            for (int d = 1 - isNum; d <= up; ++d) {
                if ((mask >> d & 1) == 0) {
                    res += dfs(i + 1, mask | 1 << d, isLimit && d == up, true);
                }
            }
            if (!isLimit && isNum) {
                cache[i][mask] = res;
            }
            return res;
        };
        return n - dfs(0, 0, true, false);
    }
};
```

> Go

```go
func numDupDigitsAtMostN(n int) int {
    s := strconv.Itoa(n)
    m := len(s)
    cache := make([][1 << 10]int, m)
    for i := range cache {
        for j := range cache[i] {
            cache[i][j] = -1
        }
    }
    var dfs func(int, int, bool, bool) int
    dfs = func(i, mask int, isLimit, isNum bool) int {
        if i == m {
            if isNum {
                return 1
            }
            return 0
        }
        if !isLimit && isNum && cache[i][mask] != -1 {
            return cache[i][mask]
        }
        res := 0
        if !isNum {
            res = dfs(i + 1, mask, false, false)
        }
        up := 9
        if isLimit {
            up = int(s[i] - '0')
        }
        low := 1
        if isNum {
            low = 0
        }
        for d := low; d <= up; d++ {
            if (mask >> d & 1) == 0 {
                res += dfs(i + 1, mask | 1 << d, isLimit && d == up, true)
            }
        }
        if !isLimit && isNum {
            cache[i][mask] = res
        }
        return res
    }
    return n - dfs(0, 0, true, false)
}
```

## :lollipop: []()

- :cherry_blossom: 思路


- **:beers: 代码**

> c++

```c++

```

> Go

```go

```

# :wink: 二叉树

## :lollipop: [112. 路径总和](https://leetcode.cn/problems/path-sum/description/)

- :cherry_blossom: 思路

一道简单题, 但是一直 wa :sob:, 忽略了叶子节点这个条件 (无左右孩子)

- **:beers: 代码**

> c++

```c++
class Solution {
public:
    bool hasPathSum(TreeNode* root, int targetSum) {
        if (!root) return false;
        int diff = targetSum - root->val;
        if (!root->left && !root->right) return diff == 0;
        return hasPathSum(root->left, diff) || hasPathSum(root->right, diff);
    }
};
```

> Go

```go
func hasPathSum(root *TreeNode, targetSum int) bool {
    if root == nil {
        return false
    }
    diff := targetSum - root.Val
    if root.Left == nil && root.Right == nil {
        return diff == 0
    }
    return hasPathSum(root.Left, diff) || hasPathSum(root.Right, diff)
}
```

## :lollipop: []()

- :cherry_blossom: 思路


- **:beers: 代码**

> c++

```c++

```

> Go

```go

```

# :wink: 图

## :lollipop: [785. 判断二分图](https://leetcode.cn/problems/is-graph-bipartite/description/)

- :cherry_blossom: 思路

染色法，相邻的点染不同的颜色，若哪个点发现它跟一个与它相邻的点的颜色一样的话，说明不能把这个图划分为两个独立子集，自己想的 BFS, DFS

还可以用并查集来做, 对于任意一个节点而言, 因为它不能跟与它相邻的节点同属于一个集合, 所以应该将所有与它相邻的节点合并为一个集合, 若发现跟它相邻的节点中存在某个节点跟它属于同一个集合的话就证明无法划分为两个独立子集

- **:beers: 代码**

> c++

```c++
// BFS
class Solution {
public:
    bool isBipartite(vector<vector<int>>& graph) {
        int n = graph.size();
        vector<int> color(n, -1);
        queue<int> q;
        for (int i = 0; i < n; ++i) {
            if (color[i] != -1) continue;
            q.emplace(i);
            color[i] = 0;
            while (!q.empty()) {
                int x = q.front(), c = color[x];
                q.pop();
                for (int &next : graph[x]) {
                    if (color[next] == -1) {
                        color[next] = 1 - c;
                        q.emplace(next);
                    } else if (color[next] == c) {
                        return false;
                    }
                }
            }
        }
        return true;
    }
};

// DFS
class Solution {
public:
    bool isBipartite(vector<vector<int>>& graph) {
        int n = graph.size();
        vector<int> color(n, -1);
        function<bool(int, int)> dfs = [&](int i, int last) -> bool {
            if (color[i] != -1) return color[i] != last;
            color[i] = 1 - last;
            for (int &next : graph[i]) {
                if (!dfs(next, color[i])) {
                    return false;
                }
            }
            return true;
        };
        for (int i = 0; i < n; ++i) {
            if (color[i] != -1) continue;
            if (!dfs(i, 0)) {
                return false;
            }
        }
        return true;
    }
};

// 并查集
class UnionFind {
public:
    vector<int> fa, size;

    UnionFind(int n) : fa(n), size(n, 1) {
        iota(fa.begin(), fa.end(), 0);
    }

    int find(int x) {
        return fa[x] == x ? x : fa[x] = find(fa[x]); 
    }

    void unite(int x, int y) {
        x = find(x);
        y = find(y);
        if (x == y) return;
        if (size[x] < size[y]) {
            swap(x, y);
        }
        fa[y] = x;
        size[x] += size[y];
    }
};

class Solution {
public:
    bool isBipartite(vector<vector<int>>& graph) {
        int n = graph.size();
        UnionFind uf(n);
        for (int i = 0; i < n; ++i) {
            if (graph[i].empty()) continue;
            for (int j = 0; j < graph[i].size(); ++j) {
                if (uf.find(i) == uf.find(graph[i][j])) return false;
                uf.unite(graph[i][0], graph[i][j]);
            }
        }
        return true;
    }
};
```

## :lollipop: [399. 除法求值](https://leetcode.cn/problems/evaluate-division/)

- :cherry_blossom: 思路

不会，**带权并查集**, ，权值为当前节点 `x` 与父亲 `f[x]` 的比值
假设两点 x 和 y 有共同的父亲f，且 `v[x] / v[f] = a, v[y] / v[f] = b`, 则 `v[x] / v[y] = a / b`

- **:beers: 代码**

> c++

```c++
class UnionFind {
public:
    vector<int> fa;
    vector<double> weight;
    //初始化时每个节点是自己的父节点，权重为1.0
    UnionFind(int n): fa(n), weight(n, 1.0) {
        iota(fa.begin(), fa.end(), 0);
    };

    // x / fx = a; fx / f = b;
    // x / f = a * b;
    int find(int x) {
        if (x == fa[x]) return x;
        int origin = fa[x];
        fa[x] = find(fa[x]);
        weight[x] = weight[x] * weight[origin];
        return fa[x];
        //无权重：retrun x == fa[x] ? x : fa[x] = find(f[x]);
        //同理还需要修改x的权重
    }

    // x / fx = a, y / f = b, x / y = c;
    // fx / f = (fx / x) / (x / f) = (1 / a) / (b * c) = b * c / a
    void Union(int x, int y, double value) {
        int rootx = find(x), rooty = find(y);
        if(rootx == rooty) return;
        fa[rootx] = rooty;
        weight[rootx] = value * weight[y] / weight[x];
        //无权重：fa[rootx] = rooty; ，仅需修改x的父节点的父节点
        //同理还需修改x的父节点的权重
    }

    // x / f = a, y / f = b
    // x / y = a / b
    double isConnect(int x, int y) {
        int rootx = find(x), rooty = find(y);
        if(rootx != rooty) return -1.0;
        else return weight[x] / weight[y];
    }
};

class Solution {
public:
    vector<double> calcEquation(vector<vector<string>>& equations, vector<double>& values, vector<vector<string>>& queries) {
        unordered_map<string, int> umap;
        int index = 0, size = values.size();
        for (auto& vs : equations) {
            if (!umap.count(vs[0])) umap[vs[0]] = index++;
            if (!umap.count(vs[1])) umap[vs[1]] = index++;
        }

        UnionFind Uf(index);
        for(int i = 0; i < size; ++i)
            Uf.Union(umap[equations[i][0]], umap[equations[i][1]], values[i]);
        vector<double> res;
        for(vector<string>& vs : queries) {
            if(!umap.count(vs[0]) || !umap.count(vs[1])) res.emplace_back(-1.0);
            else res.emplace_back(Uf.isConnect(umap[vs[0]], umap[vs[1]]));
        }
        return res;
    }
};
```

## :lollipop: []()

- :cherry_blossom: 思路


- **:beers: 代码**

> c++

```c++

```

> Go

```go

```


# :wink: 

## :lollipop: []()

- :cherry_blossom: 思路


- **:beers: 代码**

> c++

```c++

```

> Go

```go

```