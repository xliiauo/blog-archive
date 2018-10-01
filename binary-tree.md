# Binary Tree

## 二叉树遍历

* 遍历顺序 - 先中后序
* **递归方法**：
  * **递归函数** - 要能一句话说出干什么
  * **退出条件** - node is None，走到空再退出，简便
  * 耗费有限的栈空间
  * 自身无return
* **非递归方法**：
  * 背诵
  * 前序中序值得背诵
  * 递归转非递归用stack
* **分治模版**：
  * 通用 - 所有二叉树问题都可以分治疗
  * 也是递归 - 自身有return
  * **递归函数** - 一句话说清楚函数作用
  * **退出条件** - 点为空，条件提前达成
  * **分开** - 分开处理
  * **合并** - 合并处理结果
  * **返回** - 返回值，可与合并同框

### Binary Tree Traversal

#### Recursion - pre, in, post 差别不大, conquer决定顺序

```python
class Solution:
    def preorderTraversal(self, root):
        self.result = []
        self.traverse(root)
        return self.result
        
    def traverse(self, node): 
        if node is None:
            return
        
        self.result.append(node.val)
        self.traverse(node.left)    
        self.traverse(node.right)
```

#### None-recursion

**先序**

```python
class Solution:
    def preorderTraversal(self, root):
        result = []
        stack = [root]
        while stack:
            node = stack.pop()
            if node is None:
                continue
                
            result.append(node.val)
            stack.append(node.right)
            stack.append(node.left)
        return result  
```

**中序**

思路：基本靠背，非常有意思的解法

```python
class Solution:
    def inorderTraversal(self, root):
        result = []
        stack = []
        node = root
        while node or stack:
            while node:
                stack.append(node)
                node = node.left
            node = stack.pop()
            result.append(node.val)
            node = node.right
                
        return result
```

#### Divide & Conquer - pre, in, post 差别不大, conquer决定顺序 - 分治模版

```python
class Solution:
    def preorderTraversal(self, root):
        result = []
        if root is None:
            return result

        # Divide
        left = self.preorderTraversal(root.left)
        right = self.preorderTraversal(root.right)
        
        # Conquer
        result.append(root.val)
        result.extend(left)
        result.extend(right)
        
        return result
```

### Binary Search Tree Iterator

思路：Non-recursive Inorder Traversal改写

```python
class BSTIterator:
    def __init__(self, root):
        self.stack = []
        self.node = root

    def hasNext(self):
        return True if self.node or self.stack else False

    def next(self):
        while self.node:
            self.stack.append(self.node)
            self.node = self.node.left

        node = self.stack.pop()
        self.node = node.right
        
        return node
```

### Maximum Depth of Binary Tree

```python
class Solution:
    def maxDepth(self, root):
        depth = 0
        if not root:
            return depth
        else:
            depth += 1
            
        left = self.maxDepth(root.left)
        right = self.maxDepth(root.right)
        
        depth += max(left, right)
        
        return depth
```

### Balanced Binary Tree

```text
class Solution:
    def isBalanced(self, root):
        balanced, _ = self.validate(root)
        return balanced


    def validate(self, node):
        if not node:
            return True, 0
            
        balanced, leftHeight = self.validate(node.left)
        if not balanced:
            return False, 0
        balanced, rightHeight = self.validate(node.right)
        if not balanced:
            return False, 0
            
        return abs(leftHeight - rightHeight) <= 1, max(leftHeight, rightHeight) + 1
```

### Binary Tree Maximum Path Sum

```python
import math

class Solution:
    """
    @param root: The root of binary tree.
    @return: An integer
    """
    def maxPathSum(self, root):
        _, maxPath = self.helper(root)
        return maxPath
        
    def helper(self, root):
        if not root:
            return 0, -math.inf
            
        # Divide
        leftSinglePath, leftMaxPath = self.helper(root.left)
        rightSinglePath, rightMaxPath = self.helper(root.right)
        
        # Conquer
        singlePath = max(leftSinglePath, rightSinglePath) + root.val
        singlePath = max(singlePath, 0)
        
        maxPath = max(leftMaxPath, rightMaxPath)
        maxPath = max(maxPath, leftSinglePath + rightSinglePath + root.val)
        
        return singlePath, maxPath
```

### Lowest Common Acestor

```text
class Solution:
    def lowestCommonAncestor(self, root, A, B):
        if not root or root == A or root == B:
            return root
            
        # Divide
        left = self.lowestCommonAncestor(root.left, A, B)
        right = self.lowestCommonAncestor(root.right, A, B)
        
        # Conquer
        if left and right:
            return root
            
        if left:
            return left
            
        if right:
            return right
            
        return None
```

## DFS模版

```text
# Traverse
class Solution:
    def traverse(self, root):
        if not root:
            return
        # Do somehting with root
        self.traverse(root.left)
        # Do somehting with root
        self.traverse(root.right)
        # Do somehting with root
        
    
# Divide and Conquer
class Solution:
    def traverse(self, root):
        if not root:
            # Do something and return
            
        # Divide
        left = self.traverse(root.left)
        right = self.traverse(root.right)
        
        # Conquer
        result = merge result from left and right
        return result
```

## BFS模版

```text
class Solution:
    def bfs(self, root):
        rst = []
        if not root:
            return rst
            
        queue = [root]
        while queue:
            level = []
            for _ in range(len(queue)):
                node = queue.pop(0)
                level.append(node)
                
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
                    
            rst.append(level)
            
        return result
```

### Binary Tree Level Order Traversal

```text
class Solution:
    def levelOrder(self, root):
        # write your code here
        if not root: return []
        queue = [root]
        rst = []
        
        while queue:
            level = []
            for _ in range(len(queue)):
                node = queue.pop(0)
                level.append(node.val)
            
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
                    
            rst.append(level)
            
        return rst
```

### Binary Tree Zigzag Level Order Traversal

```text
class Solution:
    def zigzagLevelOrder(self, root):
        if not root: return []
        queue = [root]
        rst = []
        count = 0
        
        while queue:
            level = []
            
            for _ in range(len(queue)):
                node = queue.pop(0)
                level.append(node.val)
                
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
                    
            count += 1
            if count % 2 == 0:
                level.reverse()
            rst.append(level)
            
        return rst
```

### Validate Binary Search Tree

```text
import math

class Solution:
    """
    @param root: The root of binary tree.
    @return: True if the binary tree is BST, or false
    """
    def isValidBST(self, root):
        # write your code here
        if not root:
            return True
        
        return self.validate(root, -math.inf, math.inf)
        
    def validate(self, node, minimum, maximum):
        if not node:
            return True
            
        if node.val <= minimum or node.val >= maximum:
            return False
            
        return self.validate(node.left, minimum, min(maximum, node.val)) and self.validate(node.right, max(minimum, node.val), maximum)
```

