# 383. Ransome Note

## 题目描述

Given two strings `ransomNote` and `magazine`, return `true` *if* `ransomNote` *can be constructed by using the letters from* `magazine` *and* `false` *otherwise*.

Each letter in `magazine` can only be used once in `ransomNote`.

 

**Example 1:**

```
Input: ransomNote = "a", magazine = "b"
Output: false
```

**Example 2:**

```
Input: ransomNote = "aa", magazine = "ab"
Output: false
```

**Example 3:**

```
Input: ransomNote = "aa", magazine = "aab"
Output: true
```

 

**Constraints:**

- `1 <= ransomNote.length, magazine.length <= 105`
- `ransomNote` and `magazine` consist of lowercase English letters.



## 题解V1.0

```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        hash_ = Counter(magazine)
        for x in ransomNote:
            if hash_[x] > 0:
                hash_[x] -= 1
            else:
                return False
        return True

```

时间O(m+n)，还是用Counter把magazine中的所有字符存入计数，遍历ransomNote，若此字符在magazine中对应的值>0，就一直--，如果<=0，说明magazine中的个数不够了。

还有更简便的

```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        return not Counter(ransomNote) - Counter(magazine)
```

