# 59. Spiral Matrix II

## 题目描述

Given a positive integer `n`, generate an `n x n` `matrix` filled with elements from `1` to `n2` in spiral order.

 

**Example 1:**

<img src="./59-Spiral_Matrix.assets/spiraln.jpg" alt="img" />

```
Input: n = 3
Output: [[1,2,3],[8,9,4],[7,6,5]]
```

**Example 2:**

```
Input: n = 1
Output: [[1]]
```

 

**Constraints:**

- `1 <= n <= 20`



## 题解V1.0

```python
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        #初始化 左，右，上，下，每次更新的数num，最终数final
        #遵循不变量原则：这里我选择左闭右闭
        l, r, t, b = 0, n-1, 0, n-1
        num = 1
        matrix = [[0 for _ in range(n) ] for _ in range(n)]
        while(num <= n*n):
            #左到右 l 2 
            for i in range(l, r+1):
                matrix[t][i] = num
                num += 1
            t += 1
            #上到下 t 2 b
            for i in range(t, b+1):
                matrix[i][r] = num
                num += 1
            r -= 1
            #右到左 r 2 l
            for i in range(r, l-1, -1):
                matrix[b][i] = num
                num += 1
            b -= 1
            #下到上 b 2 t
            for i in range(b, t-1, -1):
                matrix[i][l] = num
                num += 1
            l += 1
        return matrix
```

时间复杂度O(n^2)，依照顺时针方式，从外往里填数字。选择num> n\*n的出循环条件，即不用判断奇偶数，最后num = n \* n时，会进循环填充。在每填充完数字时，都会更新上下左右的一个，保证左闭右闭没有重合。

## 题解V2.0 循环列表方法

```python
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        #左闭右开
        #初始化右，下，左，上
        step = [(0, 1), (1, 0), (0, -1), (-1, 0)]
        direction = 0
        #初始化矩阵、填充字符num
        matrix = [[0]*n for _ in range(n)]
        num = 1
        x = y = dx = dy = 0
        while(num <= n * n):
            matrix[x][y] = num
            num += 1
            #下一个位置 (x + dx, y + dy)
            nx, ny = x + step[direction][0], y + step[direction][1]
            if(nx < 0 or nx >= n or ny < 0 or ny >=n or matrix[nx][ny] != 0):
                direction = (direction+1) % 4
                nx, ny = x + step[direction][0], y + step[direction][1]
            x, y = nx, ny
        return matrix
```

y对应横轴，x对应纵轴，且右、下为正方向。

用`direction = (direction + 1) % 4`来改变方向。

填充完数字后，检查是否需要改变方向。检查越界：上下左右出界，或是下一步位置已经填充过.

不过执行时间变长了，但缩减了代码量