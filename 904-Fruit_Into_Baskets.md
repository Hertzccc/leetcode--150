# 904-Fruit into Baskets

## 题目描述

You are visiting a farm that has a single row of fruit trees arranged from left to right. The trees are represented by an integer array `fruits` where `fruits[i]` is the **type** of fruit the `ith` tree produces.

You want to collect as much fruit as possible. However, the owner has some strict rules that you must follow:

- You only have **two** baskets, and each basket can only hold a **single type** of fruit. There is no limit on the amount of fruit each basket can hold.
- Starting from any tree of your choice, you must pick **exactly one fruit** from **every** tree (including the start tree) while moving to the right. The picked fruits must fit in one of your baskets.
- Once you reach a tree with fruit that cannot fit in your baskets, you must stop.

Given the integer array `fruits`, return *the **maximum** number of fruits you can pick*.

 

**Example 1:**

```
Input: fruits = [1,2,1]
Output: 3
Explanation: We can pick from all 3 trees.
```

**Example 2:**

```
Input: fruits = [0,1,2,2]
Output: 3
Explanation: We can pick from trees [1,2,2].
If we had started at the first tree, we would only pick from trees [0,1].
```

**Example 3:**

```
Input: fruits = [1,2,3,2,2]
Output: 4
Explanation: We can pick from trees [2,3,2,2].
If we had started at the first tree, we would only pick from trees [1,2].
```

 

**Constraints:**

- `1 <= fruits.length <= 105`
- `0 <= fruits[i] < fruits.length`



## 题解V1.0-滑动窗口

```python
class Solution:
    def totalFruit(self, fruits: List[int]) -> int:
        #滑动窗口
        #滑动窗口起点i终点j（包括
        #哈希表来储存数量和种类，{种类：数量,...}
        #窗口内保持2种水果
        #初始化i,j,baskets,max	
        i = 0 
        baskets = {}
        max_ = 0
        for j in range(len(fruits)):
            if fruits[j] not in baskets:
                baskets[fruits[j]] = 0
            baskets[fruits[j]] += 1
            while(len(baskets) > 2): #篮子里有两种水果以上
                baskets[fruits[i]] -= 1
                if(baskets[fruits[i]] <= 0):
                    baskets.pop(fruits[i])
                i += 1
            total = sum(baskets.values())
            max_ = max_ if max_ >= total else total

        return max_
```

时间O(N)，此题依然是滑动窗口，只要保持两种水果在窗口内。此题用了哈希表来储存种类和数量，数量必要是因为方便去掉一类水果

## 题解V1.2 滑动窗口改进

```python
class Solution:
    def totalFruit(self, fruits: List[int]) -> int:
        #滑动窗口
        #滑动窗口起点i终点j（包括
        #哈希表来储存数量和种类，{种类：数量,...}
        #窗口内保持2种水果
        #初始化i,j,baskets,max
        i = 0 
        baskets = {}
        max_ = -1
        for j in range(len(fruits)):
            if fruits[j] not in baskets:
                baskets[fruits[j]] = 0
            baskets[fruits[j]] += 1
            while(len(baskets) > 2): #篮子里有两种水果以上
                baskets[fruits[i]] -= 1
                if(baskets[fruits[i]] <= 0):
                    baskets.pop(fruits[i])
                i += 1
            total = j - i + 1
            max_ = max_ if max_ >= total else total

        return max_
```

改进不用每次都用sum计算水果数量，total = j - i + 1



## 题解V1.3 滑动窗口引入Counter

```python
class Solution:
    def totalFruit(self, fruits: List[int]) -> int:
        #滑动窗口
        #滑动窗口起点i终点j（包括
        #哈希表来储存数量和种类，{种类：数量,...},这里模仿答案Counter
        #窗口内保持2种水果
        #初始化i,j,baskets,max
        i = 0 
        baskets = Counter()
        max_ = 0
        for j, x in enumerate(fruits):
            baskets[x] += 1
            while(len(baskets) > 2): #篮子里有两种水果以上
                baskets[fruits[i]] -= 1
                if(baskets[fruits[i]] <= 0):
                    baskets.pop(fruits[i])
                i += 1
            # total = j - i + 1 这里省略total
            max_ = max_ if max_ >= j - i + 1 else j - i + 1
        return max_
```

引入Counter()来计数，省略判断条件，直接++
