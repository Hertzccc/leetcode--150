# 54. Spiral Matrix I

## 题目描述

Given an `m x n` `matrix`, return *all elements of the* `matrix` *in spiral order*.

 

**Example 1:**

<img src="./54-%20Spiral_Matrix_I.assets/spiral1.jpg" alt="img" />

```
Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
Output: [1,2,3,6,9,8,7,4,5]
```

**Example 2:**

<img src="./54-%20Spiral_Matrix_I.assets/spiral.jpg" alt="img" />

```
Input: matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
Output: [1,2,3,4,8,12,11,10,9,5,6,7]
```

 

**Constraints:**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 10`
- `-100 <= matrix[i][j] <= 100`



## 题解V1.0 循环列表

```python
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        #遵循不变量原则，设置左闭右开
        m, n = len(matrix), len(matrix[0])
        visited = [[False]*n for _ in range(m)]
        #定义右，下，左，上
        step = [(0, 1), (1, 0), (0, -1), (-1, 0)]
        direction = 0
        x, y, i = 0, 0, 0
        A=[]
        while(i < m*n):
            visited[x][y] = True
            A.append(matrix[x][y])
            i += 1

            nx, ny = x + step[direction][0], y + step[direction][1]
            if(nx < 0 or nx >= m or ny <0 or ny >= n or visited[nx][ny]):
                direction = (direction + 1) % 4
                nx, ny = x + step[direction][0], y + step[direction][1]
            x, y = nx, ny

        return A
```

时间O(n^2)，和59题学到的循环列表一样，用step，和direction调整方向。调整方向的条件需要更改，加一个记录访问过元素的数组，当访问过对应元素，更改对应位置为True。



## 题解V2.0 

```python
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        #遵循不变量原则，设置左闭右闭
        #初始化上下左右
        m, n = len(matrix), len(matrix[0])
        l, r, t, b = 0, n-1, 0, m-1
        A = []
        num = 1
        while(num <= m * n): #共m*n个数
            #l 2 r
            for i in range(l, r+1):
                A.append(matrix[t][i])
                num += 1
            t += 1
            #t 2 b
            for i in range(t, b+1):
                A.append(matrix[i][r])
                num += 1
            r -= 1
            #r 2 l 注意逆序，且不包括最左边
            for i in range(r, l-1, -1):
                A.append(matrix[b][i])
                num += 1
            b -= 1
            #b 2 r
            for i in range(b, t-1, -1):
                A.append(matrix[i][l])
                num += 1
            l += 1
        return A
```

这个边界值一直有问题，应该改为每次都判断是否超过边界，因为不是n*n的数组。

在解法1.0中，因为有visited来确保只访问过一次。

```python
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        #遵循不变量原则，设置左闭右闭
        #初始化上下左右
        m, n = len(matrix), len(matrix[0])
        l, r, t, b = 0, n-1, 0, m-1
        A = []
        num = 1
        while(True): #共m*n个数
            #l 2 r
            for i in range(l, r+1):
                A.append(matrix[t][i])
            t += 1
            if(t > b): break
            #t 2 b
            for i in range(t, b+1):
                A.append(matrix[i][r])
            r -= 1
            if(l > r ): break
            #r 2 l 注意逆序，且不包括最左边
            for i in range(r, l-1, -1):
                A.append(matrix[b][i])
            b -= 1
            if(t > b): break
            #b 2 r
            for i in range(b, t-1, -1):
                A.append(matrix[i][l])
            l += 1
            if(l > r): break
        return A
```

