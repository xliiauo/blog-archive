# Tree Traversals

```ruby
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

def in_order node
  unless node.nil?
    in_order node.left
    puts node.value
    in_order node.right
  end
end

def pre_order node
  unless node.nil?
    puts node.value
    pre_order node.left
    pre_order node.right
  end
end

def post_order node
  unless node.nil?
    post_order node.left
    post_order node.right
    puts node.value
  end
end

tree = Tree.new Node.new(1)
tree.root.left = Node.new(0)
tree.root.right = Node.new(2)

in_order tree.root
pre_order tree.root
post_order tree.root
```

