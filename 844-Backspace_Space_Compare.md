# 844. Backspace space compare

## 题目描述

Given two strings `s` and `t`, return `true` *if they are equal when both are typed into empty text editors*. `'#'` means a backspace character.

Note that after backspacing an empty text, the text will continue empty.

 

**Example 1:**

```
Input: s = "ab#c", t = "ad#c"
Output: true
Explanation: Both s and t become "ac".
```

**Example 2:**

```
Input: s = "ab##", t = "c#d#"
Output: true
Explanation: Both s and t become "".
```

**Example 3:**

```
Input: s = "a#c", t = "b"
Output: false
Explanation: s becomes "c" while t becomes "b".
```

 

**Constraints:**

- `1 <= s.length, t.length <= 200`
- `s` and `t` only contain lowercase letters and `'#'` characters.

 

**Follow up:** Can you solve it in `O(n)` time and `O(1)` space?



## 题解V1.0-双指针

```python
class Solution:
    def backspaceCompare(self, s: str, t: str) -> bool:
        #双指针
        i = len(s)-1
        j = len(t)-1
        count = 0
        while(i>=0 or j>=0):
            #检测到#，继续回退#个数
            while(i >=0):
                if s[i] == "#":
                    i -= 1
                    count += 1
                elif count > 0: #存在还可以跳过的值
                    i -= 1
                    count -= 1
                else: #找到可以比较的值或到头了
                    break
            count = 0
            while(j >=0):
                if t[j] == "#":
                    j -= 1
                    count += 1
                elif count > 0: #存在还可以跳过的值
                    j -= 1
                    count -= 1
                else: #找到可以比较的值或到头了
                    break

            if(i >= 0 and j >= 0): # 两个都没到头
                if(s[i] != t[j]): #比较对应值，对应上了直接跳到递减环节，elif的重要性
                    return False
            elif(i >= 0 or j >= 0): #有一个到头了，另一个没到头
                return False
            i -= 1
            j -= 1
        return True
```

时间O(N)，两个指针分别指向两个字符串s,t末尾。从后向前遍历，每个指针碰到"#"时，都要记录#的数量，当数量为0还有不是#时，就是找到了可以进行比较的字符串。大循环的条件设置的是or，是因为如果一方到头，会直接返回匹配成功。

举例：s:"bbbextm"，t:"bbb#extm"。这里s比t多一个字符，显然多的是中间#，会导致匹配失败。但如果将大循环条件设置成and，那么会导致s结束时，匹配成功，不正确。

大循环条件or保证了 两个字符串都会跑到最后一个值（如果没有提前匹配失败）。

