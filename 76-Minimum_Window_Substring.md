# 76. Minimum Window Substring

## 题目描述

Given two strings `s` and `t` of lengths `m` and `n` respectively, return *the **minimum window*** 

***substring\***

 *of* `s` *such that every character in* `t` *(**including duplicates**) is included in the window*. If there is no such substring, return *the empty string* `""`.



The testcases will be generated such that the answer is **unique**.

 

**Example 1:**

```
Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.
```

**Example 2:**

```
Input: s = "a", t = "a"
Output: "a"
Explanation: The entire string s is the minimum window.
```

**Example 3:**

```
Input: s = "a", t = "aa"
Output: ""
Explanation: Both 'a's from t must be included in the window.
Since the largest window of s only has one 'a', return empty string.
```

 

**Constraints:**

- `m == s.length`
- `n == t.length`
- `1 <= m, n <= 105`
- `s` and `t` consist of uppercase and lowercase English letters.

 

**Follow up:** Could you find an algorithm that runs in `O(m + n)` time?



## 题解V1.0 滑动窗口

```python
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        #初始化
        # min_info = {'min': float('inf'), 'index': -1} # 存储相应的substring长度和对应起始位置
        ans_l, ans_r = -1, len(s)
        min_ =  float('inf')
        if (len(s) < len(t)):
            return ""
        t_hash = Counter(t) #t存进哈希中，用来做基准,不会变
        s_hash = Counter() #s中的哈希,用来比较
        i = 0 # 左端点 窗口内保持有t的所有值

        for j, x in enumerate(s):
            if x in t_hash:
                s_hash[x] += 1
            
            #窗口内的值大于
            while(self.isLarge(t_hash,s_hash)):
                if s[i] in t_hash:
                    s_hash[s[i]] -= 1

                if j-i+1 <= min_:
                    ans_l, ans_r = i, j
                    min_ = j-i+1
                i += 1
            
        
        return s[ans_l:ans_r+1] if min_ != float('inf') else ""
    

    #用来判断 1.s_hash中的是否比t_hash中的值都要大，
    #        2.s_hash中是否有 t_hash中的值
    def isLarge(self, t_hash, s_hash): 
        for k, v in t_hash.items():
            if k not in s_hash or s_hash[k] < v:
                return False
        return True  
```

这题与904很像，都用了哈希来存储值和其对应出现个数。

用到了两个哈希，一个是原始的t字符串每个char对应的出现个数。t_hash

另一个是s中与t相同的hash，用来捕捉在窗口移动时比t_hash元素大时，移动左窗口i的时机。



思考：

- 在判断左窗口右移时，i有没有可能>j(右窗口)?

  没有可能。i == j的话，target必须为1个char的长度，若i>j其实就没找到或者是空串

- 在判断窗口内的值是否更大更多，抽象出了一个新方法，用来判断在键key对应上的情况下，对应value数值是否比原始大，大的话才需要移动左窗口

不过每次判断是否“涵盖”，都需要判断是否存在在t_hash中（isLarge方法），若能优化到O(1)，就更好了



## 题解V1.1 滑动窗口（优化掉isLarge方法）

```python
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        #初始化
        ans_l, ans_r = -1, len(s) #存储最终答案的左右边界
        min_ =  float('inf') # 初始化最小框为inf
        if (len(s) < len(t)):
            return ""
        t_hash = Counter(t) #t存进哈希中，用来做基准,不会变
        s_hash = Counter() #s中的哈希,用来比较
        i = 0 # 左端点 窗口内保持有t的所有值

        for j, x in enumerate(s):
            if x in t_hash:
                s_hash[x] += 1
            
            #窗口内的值大于
            while all(s_hash[c] >= t_hash[c] for c in t_hash):
                if s[i] in t_hash:
                    s_hash[s[i]] -= 1

                if j-i+1 <= min_:
                    ans_l, ans_r = i, j
                    min_ = j-i+1
                i += 1   
        return s[ans_l:ans_r+1] if min_ != float('inf') else ""
```

不过leetcode判断用时好像变慢了，当然还可以替换成，经实测又要慢100~200ms

```python
while s_hash >= t_hash:
```



## 题解V1.2 优化掉min_

```python
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        #初始化
        ans_l, ans_r = -1, len(s) #存储最终答案的左右边界
        # min_ =  float('inf') # 初始化最小框为inf
        if (len(s) < len(t)):
            return ""
        t_hash = Counter(t) #t存进哈希中，用来做基准,不会变
        s_hash = Counter() #s中的哈希,用来比较
        i = 0 # 左端点 窗口内保持有t的所有值

        for j, x in enumerate(s):
            if x in t_hash:
                s_hash[x] += 1
            
            #窗口内的值大于等于
            while all(s_hash[c] >= t_hash[c] for c in t_hash):
                if s[i] in t_hash:
                    s_hash[s[i]] -= 1

                if j-i+1 < ans_r - ans_l + 1:
                    ans_l, ans_r = i, j
                    min_ = j-i+1
                i += 1   
        return "" if ans_l < 0 else s[ans_l: ans_r+1]
```

因为min_的当前值就是可以用ans_r-ans_l来表示



## 题解V2.0 灵神的解法-省略每次移入子串的判断

```python
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        ans_left, ans_right = -1, len(s)
        left = 0
        cnt_s = Counter()  # s 子串字母的出现次数
        cnt_t = Counter(t)  # t 中字母的出现次数
        less = len(cnt_t)  # 有 less 种字母的出现次数 < t 中的字母出现次数
        for right, c in enumerate(s):  # 移动子串右端点
            cnt_s[c] += 1  # 右端点字母移入子串
            if cnt_s[c] == cnt_t[c]:
                less -= 1  # c 的出现次数从 < 变成 >=
            while less == 0:  # 涵盖：所有字母的出现次数都是 >=
                if right - left < ans_right - ans_left:  # 找到更短的子串
                    ans_left, ans_right = left, right  # 记录此时的左右端点
                x = s[left]  # 左端点字母
                if cnt_s[x] == cnt_t[x]:
                    less += 1  # x 的出现次数从 >= 变成 <（下一行代码执行后）
                cnt_s[x] -= 1  # 左端点字母移出子串
                left += 1  # 移动子串左端点
        return "" if ans_left < 0 else s[ans_left: ans_right + 1]
```

这里用了less变量来维护：在s中有less种字母出现的次数小于t中字母出现的次数

即在第7行，less初始化为t中字母出现的次数。

enumerate s时，右端点对应的c移入子串，比较c在s中的数量是否对应上在t中的数量。对应上了就less--，所以是刚好对应上，就算数量超了less也不会变化。

之后看less是否正好是0，即是否涵盖。之前判断涵盖是用了两个Counter（不过这里是否也不需要两个Counter的必要？如果可以，我会更新）

### 仿照灵神修改的代码

```python
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        #初始化
        ans_l, ans_r = -1, len(s) #存储最终答案的左右边界
        if (len(s) < len(t)):
            return ""
        t_hash = Counter(t) #t存进哈希中，用来做基准,不会变
        s_hash = Counter() #s子串中的哈希,用来比较
        less = len(t_hash)
        i = 0 # 左端点 窗口内保持有t的所有值

        for j, x in enumerate(s):
            s_hash[x] += 1 #直接移入子串
            if s_hash[x] == t_hash[x]:
                less -= 1
            #less-到0，其实就是涵盖到，所有字母出现次数>=
            while less == 0:
                if j-i+1 < ans_r - ans_l + 1:
                    ans_l, ans_r = i, j
                #s[i]左端点的字母
                if(s_hash[s[i]] == t_hash[s[i]]): 
                    #因为左端点要右移，如果移除的是t_hash中的，
                    #应让窗口内保持子串字母不够的状态(未涵盖)，less++
                    less += 1 
                s_hash[s[i]] -= 1 #左端点移除
                i += 1   
        return "" if ans_l < 0 else s[ans_l: ans_r+1]
```



## 题解V2.1 优化掉一个Counter

```python
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        m = len(s)
        cnt = Counter(t)
        left = cur = 0
        ans = "0" * (m+1)
        for right, c in enumerate(s):
            #处理右侧新加字符
            cnt[c] -= 1
            if cnt[c] >= 0:
                cur += 1
            # 收缩左边，去掉不在t的字符和超过出现在t次数的字符
            while left < m and cnt[s[left]] < 0:
                cnt[s[left]] += 1
                left += 1
            if cur == len(t) and right-left+1 < len(ans):
                ans = s[left:right+1]
        return ans if len(ans) < m + 1 else ""
```

灵神下面的评论区的答案，这个的cur正好与less相反，cur是从0开始往上加到与t中字符数量相等，less是--。

只用了一个Counter维护，右侧窗口拓展时，直接cnt[c]--

### 改成自己易于理解的代码：

```python
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        m = len(s)
        t_hash = Counter(t)
        i = cur = 0
        ans = "0" * (m+1)
        for j, c in enumerate(s):
            #处理右侧新加字符
            t_hash[c] -= 1
            if t_hash[c] >= 0:
                cur += 1
            # 收缩左边，去掉不在t的字符和超过出现在t次数的字符
            while i < m and t_hash[s[i]] < 0:
                t_hash[s[i]] += 1
                i += 1
            if cur == len(t) and j-i+1 < len(ans):
                ans = s[i:j+1]
        return ans if len(ans) < m + 1 else ""
```

t_hash[s[i]]减到0时，恰好在t中对应的s[i]字符次数正好对应，如果小于0，说明超过了t出现的次数，需要收缩左窗口