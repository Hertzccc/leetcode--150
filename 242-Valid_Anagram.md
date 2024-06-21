# 242. Valid Anagram

## 题目描述

Given two strings `s` and `t`, return `true` *if* `t` *is an anagram of* `s`*, and* `false` *otherwise*.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

 

**Example 1:**

```
Input: s = "anagram", t = "nagaram"
Output: true
```

**Example 2:**

```
Input: s = "rat", t = "car"
Output: false
```

 

**Constraints:**

- `1 <= s.length, t.length <= 5 * 104`
- `s` and `t` consist of lowercase English letters.

 

**Follow up:** What if the inputs contain Unicode characters? How would you adapt your solution to such a case?



## 题解V1.0 数组当字典 java

```java
class Solution {
    public boolean isAnagram(String s, String t) {
        if(s.length() != t.length()){
            return false;
        }
        int[] hash_ = new int[26];
        for(int i=0; i<s.length();i++){
            hash_[s.charAt(i) - 'a'] ++;
            hash_[t.charAt(i) - 'a'] --;
        }
        for(int i=0; i<26;i++){
            if(hash_[i] != 0){
                return false;
            }
        }
        return true;
    }
}
```



## 题解V2.0 用hashmap java

```java
class Solution {
    public boolean isAnagram(String s, String t) {
        if(s.length() != t.length()){
            return false;
        }
        Map<Character, Integer> hash_ = new HashMap<Character, Integer>();
        for(int i=0; i<s.length();i++){
            hash_.put(s.charAt(i), hash_.getOrDefault(s.charAt(i),0) + 1);
            hash_.put(t.charAt(i), hash_.getOrDefault(t.charAt(i),0) - 1);
        }
        for(int v: hash_.values()){
            if (v < 0){ return false;}
        }
        return true;
    }
}
```



## 题解V3.0 python Counter

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if(len(s) != len(t)):
            return False
        hash_t = Counter(s)
        hash_s = Counter(t)
        if hash_s != hash_t:
            return False
        return True
```

