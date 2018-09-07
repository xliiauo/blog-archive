# Subsets & Permutations

## 排列组合模版

* 先画图，模拟构造
* 所有搜索，但是要根据情况改动
* 根据实际情况改动
  * 什么时候输出
  * 哪些情况跳过

### Subsets

```ruby
def subsets(nums)
    results = []
    list = []
    subsets_helper(results, list, nums, 0)
    results
end

def subsets_helper(results, list, nums, pos)
    results << list.dup
    
    (pos...nums.length).each do |i|
        list << nums[i]
        subsets_helper(results, list, nums, i + 1)
        list.pop
    end
end
```

### Subsets II

```ruby
def subsets_with_dup(nums)
    results = []
    list = []
    nums.sort!
    subsets_helper(results, list, nums, 0)
    results
end

def subsets_helper(results, list, nums, pos)
    results << list.dup
    
    (pos...nums.length).each do |i|
        next if nums[i] == nums[i - 1] && pos != i
        list << nums[i]
        subsets_helper(results, list, nums, i + 1)
        list.pop
    end
end
```

### Permutations

```ruby
def permute(nums)
    results = []
    list = []
    permute_helper(results, list, nums)
    results
end

def permute_helper(results, list, nums)
    results << list.dup if list.length == nums.length
    
    (0...nums.length).each do |i|
        next if list.include? nums[i]
        list << nums[i]
        permute_helper(results, list, nums)
        list.pop
    end
end 
```

### Permutations II

```ruby
def permute_unique(nums)
    results = []
    list = []
    index = []
    nums.sort!
    permute_helper(results, list, nums, index)
    results
end

def permute_helper(results, list, nums, index)
    results << list.dup if list.length == nums.length
    
    (0...nums.length).each do |i|
        next if index.include?(i) || i > 0 && (nums[i] == nums[i - 1] && !index.include?(i - 1))
        list << nums[i]
        index << i
        permute_helper(results, list, nums, index)
        list.pop
        index.pop
    end
end
```

### Combinations

```ruby
def combine(n, k)
    results = []
    list = []
    combine_helper(results, list, n, 1, k)
    results
end

def combine_helper(results, list, n, pos, k)
    results << list.dup if list.length == k
    
    (pos..n).each do |i|
        next if list.length == k
        list << i
        combine_helper(results, list, n, i + 1, k)
        list.pop
    end
end
```

