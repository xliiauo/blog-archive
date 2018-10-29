# Dynamic Programming

## **动态规划模版**

* 状态定义 State - 自然语言描述数组的定义
* 初始化 Initialisation - 起点，最小状态
* 方程 Function - 根据定义写出方程，状态之间的关系，小状态找到大状态 \(离起点越近，状态越小\)
* 答案 Answer - 终点，最大状态

**何时使用DP?**

* 问：max/min\(最大最小值\)， yes/no \(路径\)， count \(多少种走法\)
* Can't sort/swap, 例如给集合不是list极有可能不是动态规划

#### 类别

* 矩阵
  * state - f\[x\]\[y\] - 从起点走到x, y
  * function - 走到x,y前一步
  * initial - 起点
  * answer - 终点
* 


### Triangle

#### DFS - O\(2^n\), unlike binary tree's O\(n\)

```python
import math

class Solution:
    def minimumTotal(self, triangle):
        self.triangle = triangle
        self.best = math.inf
        self.dfs(0, 0, 0)
        return self.best
        
    def dfs(self, x, y, s):
        if  x == len(self.triangle):
            self.best = min(self.best, s)
            return
            
        self.dfs(x + 1, y, s + self.triangle[x][y])
        self.dfs(x + 1, y + 1, s + self.triangle[x][y])
```

#### Divide & Conquer

```python
import math

class Solution:
    def minimumTotal(self, triangle):
        self.triangle = triangle
        return self.dfs(0, 0)
            
    # x, y出发走到底层所能到达的最小路径的和
    def dfs(self, x, y):
        if x == len(self.triangle):
            return 0
            
        return min(self.dfs(x + 1, y), self.dfs(x + 1, y + 1)) + self.triangle[x][y]
```

#### Divide & Conquer + 记忆化搜索

思路：脑子存在就返回脑子，不存在就记录进脑子

```python
import math

class Solution:
    def minimumTotal(self, triangle):
        self.triangle = triangle
        self.dic = {}
        return self.dfs(0, 0)
            
            
    def dfs(self, x, y):
        if x == len(self.triangle):
            return 0
            
        if self.dic.get((x, y)):
            return self.dic[(x, y)]
            
        self.dic[(x, y)] = min(self.dfs(x + 1, y), self.dfs(x + 1, y + 1)) + self.triangle[x][y]
        return self.dic[(x, y)]
```

#### DP - 两重循环

自底向上/自顶向下

注意：`if [variable]` 可能并不能判断是否存在，因为`0`是`False`

```python
class Solution:
    def minimumTotal(self, triangle):
      f = {}
      f[(0, 0)] = triangle[0][0]
    
      for i in range(1, len(triangle)):
    
        for j in range(0, i + 1):
          f[(i, j)] = math.inf
          if f.get((i - 1, j)) is not None:
            f[(i, j)] = min(f[(i, j)], f[(i - 1, j)])
          if f.get((i - 1, j - 1)) is not None:
            f[(i, j)] = min(f[(i, j)], f[(i - 1, j - 1)])
          f[(i, j)] += triangle[i][j]
    
      crr = math.inf
      for i in range(0, len(triangle)):
        if f[(len(triangle) - 1, i)] < crr:
            crr = f[(len(triangle) - 1, i)]
      return crr
```

Minimum Path Sum



