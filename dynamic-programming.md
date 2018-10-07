# Dynamic Programming

Triangle

DFS - O\(2^n\), unlike binary tree's O\(n\)

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

Divide & Conquer

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

DP - 记忆化搜索

脑子存在就返回脑子，不存在就记录进脑子

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

DP - 两重循环



