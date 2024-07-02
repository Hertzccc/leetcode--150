# 1047. Remove all adjacent duplicates in  String

## 题目描述

You are given a string `s` consisting of lowercase English letters. A **duplicate removal** consists of choosing two **adjacent** and **equal** letters and removing them.

We repeatedly make **duplicate removals** on `s` until we no longer can.

Return *the final string after all such duplicate removals have been made*. It can be proven that the answer is **unique**.

 

**Example 1:**

```
Input: s = "abbaca"
Output: "ca"
Explanation: 
For example, in "abbaca" we could remove "bb" since the letters are adjacent and equal, and this is the only possible move.  The result of this move is that the string is "aaca", of which only "aa" is possible, so the final string is "ca".
```

**Example 2:**

```
Input: s = "azxxzy"
Output: "ay"
```

 

**Constraints:**

- `1 <= s.length <= 105`
- `s` consists of lowercase English letters.

## 题解V1.0 用Stack类

```java
class Solution {
    public String removeDuplicates(String s) {
        if(s.length() == 0) {
            return new String("");}
        Stack<Character> stack = new Stack<>();
        for (Character c: s.toCharArray()){
            //如果stack里没有元素 或者 
            if(stack.isEmpty() || stack.peek() != c){
                stack.push(c);
            }else if(!stack.isEmpty()){
                stack.pop();
            }
        }
        String str = stack.stream()
                      .map(Object::toString)
                      .collect(Collectors.joining());
        // for (Character c : stack) {
        //     str.append(c);
        // }
        // return str.toString();
        return str;
    }
}
```



## 题解V2.0 用deque

```java
class Solution {
    public String removeDuplicates(String s) {
        if(s.length() == 0) {
            return new String("");}
        Deque<Character> stack = new ArrayDeque<>();
        for (Character c: s.toCharArray()){
            //如果stack里没有元素 或者 
            if(stack.isEmpty() || stack.peek() != c){
                stack.push(c);
            }else if(!stack.isEmpty()){
                stack.pop();
            }
        }
        StringBuilder result = new StringBuilder();
        for (Iterator<Character> it = stack.descendingIterator(); it.hasNext(); ) {
            result.append(it.next());
        }

        return result.toString();
    }
}
```

