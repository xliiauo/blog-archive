# Binary Tree

## 二叉树遍历



## DFS模版

## BFS模版



### Binary Tree Preorder Traversal

#### Recursion

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

#### Divide & Conquer

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

