# 151. Reverse words in a string

## 题目描述

Given an input string `s`, reverse the order of the **words**.

A **word** is defined as a sequence of non-space characters. The **words** in `s` will be separated by at least one space.

Return *a string of the words in reverse order concatenated by a single space.*

**Note** that `s` may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.

 

**Example 1:**

```
Input: s = "the sky is blue"
Output: "blue is sky the"
```

**Example 2:**

```
Input: s = "  hello world  "
Output: "world hello"
Explanation: Your reversed string should not contain leading or trailing spaces.
```

**Example 3:**

```
Input: s = "a good   example"
Output: "example good a"
Explanation: You need to reduce multiple spaces between two words to a single space in the reversed string.
```

 

**Constraints:**

- `1 <= s.length <= 104`
- `s` contains English letters (upper-case and lower-case), digits, and spaces `' '`.
- There is **at least one** word in `s`.

 

**Follow-up:** If the string data type is mutable in your language, can you solve it **in-place** with `O(1)` extra space?



## 题解V1.0-双指针

```python
class Solution:
    def reverseWords(self, s: str) -> str:
        #初始化
        s = s.strip()
        ans = []
        i = j = len(s) - 1 #快指针i，慢指针j
        #逆序遍历
        while(i >= 0):
            while(i >= 0 and s[i] != ' '): # 快指针
                i -= 1          #找到第一个单词左边界(此时已经超了边界)
            ans.append(s[i+1:j+1])
            while(i >= 0 and s[i] == ' '): #
                i -= 1          #跳过空格
            j = i               #确定新单词的右边界
        return " ".join(ans)
```

时间O(N)

逆序遍历，找到单词添加进列表，最后转成字符串

初始化后，右边界是j，用i找到单词左边界，找到单词添加。为下一次添加单词做准备，i继续向左遍历，找到单词的右边界。要注意的是，i总是超过左边界一位



## 题解V2.0-python内置方法

```python
class Solution:
    def reverseWords(self, s: str) -> str:
        s = s.strip() #去除头尾空格
        s = s.split() #分割字符串，成列表
        s = reversed(s)   #翻转单词列表
        #s.reverse()
        return ' '.join(s) 
```

应用python的内置方法，

- `strip()`去除头尾空格
- `spilt()`分割字符串，返回列表
- `reversed(s)`翻转列表



## 题解V2.1-模拟python内置方法

```python
class Solution:
    def reverseWords(self, s: str) -> str:
        #先strip
        #split分割
        #逆序
        s = self.strip_(s)
        str_l = self.split_(s)
        str_l = self.reverse_(str_l)
        return ' '.join(str_l)

    def strip_(self, s:str) -> str:
        i, j = 0, len(s)-1
        #去除头尾空格。
        while(i <= j and s[i] == ' '):
            i += 1
        while(i <= j and s[j] == ' '):
            j -= 1
        return s[i:j+1]


    def split_(self, s:str) -> list[str]:
        #初始化
        ans = []
        i = j = 0 #i是左边界，j是右边界
        while(j < len(s)):
            while(j < len(s) and s[j] != ' '):
                j += 1
            ans.append(s[i:j])
            while(j < len(s) and s[j] == ' '):
                j += 1
            i = j
        return ans
    def reverse_(self, list_s:list[str]) -> list[str]:
        l, r = 0, len(list_s)-1 #左闭右闭
        while(l <= r):
            list_s[l], list_s[r] = list_s[r], list_s[l]
            l, r = l+1, r-1
        return list_s
```

有点麻烦，我觉得没有必要，其中`split()`我写的和V1.0双指针差不多，从头开始遍历