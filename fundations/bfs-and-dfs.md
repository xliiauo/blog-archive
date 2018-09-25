# BFS & DFS

```ruby
# Prepare a sample tree
class Node
  attr_accessor :value, :left, :right
  
  def initialize value
    @value = value
    @left = nil
    @right = nil
  end
end

class Tree
  attr_accessor :root
  
  def initialize node
    @root = node
  end
end

tree = Tree.new Node.new 0
tree.root.left = Node.new 1
tree.root.right = Node.new 2
tree.root.left.left = Node.new 3

# DFS - recursive
def dfs node
  return if node.nil?
  
  puts node.value
  
  dfs node.left unless node.left.nil?
  dfs node.right unless node.right.nil?
end

dfs tree.root

# BFS - queue
def bfs node
  q = Queue.new
  return if node.nil?
  q.enq node
  
  while !q.empty?
    node = q.deq
    puts node.value
    
    q.enq node.left unless node.left.nil?
    q.enq node.right unless node.right.nil?
  end
end

bfs tree.root
```

