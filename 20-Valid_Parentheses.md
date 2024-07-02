# 20. Valid Parentheses

## 题目描述

Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.

 

**Example 1:**

```
Input: s = "()"
Output: true
```

**Example 2:**

```
Input: s = "()[]{}"
Output: true
```

**Example 3:**

```
Input: s = "(]"
Output: false
```

 

**Constraints:**

- `1 <= s.length <= 104`
- `s` consists of parentheses only `'()[]{}'`.



## 题解V1.0 栈

```java
class Solution {
    public boolean isValid(String s) {
        //每次压入左边，比较右边
        Stack<Character> left = new Stack<Character>();
      	
        for(int i=0; i<s.length();i++){
            if(s.charAt(i) == '('){
                left.push(')');
            }else if(s.charAt(i) == '{'){
                left.push('}');
            }else if(s.charAt(i) == '['){
                left.push(']');
            }
            else{
                if (left.isEmpty() || s.charAt(i) != left.pop()){
                    return false;
                }
            }
        }
        return left.isEmpty();
    }
}
```

时间O(N)，思路是如果是左括号，压右括号入栈。

如果不是左括号，判断栈内是否有对应左括号

最后return处理的是全左括号的情况

## 题解V1.1 优化

```java
class Solution {
    public boolean isValid(String s) {
        //每次压入左边，比较右边
        Stack<Character> left = new Stack<Character>();
        left.push('?');
      	
        for(int i=0; i<s.length();i++){
            if(s.charAt(i) == '('){
                left.push(')');
            }else if(s.charAt(i) == '{'){
                left.push('}');
            }else if(s.charAt(i) == '['){
                left.push(']');
            }
            else{
                if (s.charAt(i) != left.pop()){
                    return false;
                }
            }
        }
        return left.size() == 1;
    }
}
```

提前压入栈内一个?，少一步判断。也不需要判断是否为空，直接判断最后的数量是否是1.

## 题解V2.0 栈和hashmap

```java
class Solution {
    public boolean isValid(String s) {
        //构造一个哈希表用来储存括号对，一个Stack用来读入String
        Map<Character, Character> map = new HashMap<>();
        LinkedList<Character> stack = new LinkedList<>();
        map.put('(',')');map.put('[',']');
        map.put('{','}');map.put('?','?');
        stack.push('?');
        for (Character c : s.toCharArray()){
            if(map.containsKey(c)){
                stack.push(c);
            }else if (map.get(stack.pop()) != c) { return false;}
        }
        return stack.size() == 1;
    }
}
```

用hashmap来储存括号对。
