# Binary Tree

## 二叉树遍历

* 遍历顺序 - 先中后序
* **递归方法**：
  * 递归函数 - 要能一句话说出干什么
  * 退出条件 - node is None，走到空再退出，简便
  * 耗费有限的栈空间
  * 自身无return
* **非递归方法**：
  * 背诵
  * 前序中序值得背诵
  * 递归转非递归用stack
* **分治模版**：
  * 通用 - 所有二叉树问题都可以分治疗
  * 也是递归 - 自身有return

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

**中序 - 待背**

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

## DFS模版

## BFS模版

