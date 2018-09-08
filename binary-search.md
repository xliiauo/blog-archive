# Binary Search

## 二分查找模版

* 牢记四要素
  * `start + 1 < end` - 【结束条件】头尾指针相交结束，防止死循环
  * `mid = start + (end - start)/2` - 【加分项】考虑到溢出
  * `nums[mid] == < >`, `start = mid`, `end = mid` - 【指针变化】不会漏解
  * `nums[start]`, `nums[end]`, `target` - 【结果选取】由first, last, any灵活选取
* 注意末尾结果选取，一旦第一个符合条件需立刻返回，避免结果改写

### Binary Search - any

```ruby
def search(nums, target)
    return -1 if nums.empty?
    
    first = 0
    last = nums.length - 1
    
    while first + 1 < last
        mid = first + (last - first)/2
        return mid if nums[mid] == target
        first = mid if nums[mid] < target
        last = mid if nums[mid] > target
    end
    
    return first if nums[first] == target
    return last if nums[last] == target
    
    return -1
end
```

### Search for a Range

```ruby
def search_range(nums, target)
    return [-1, -1] if nums.length == 0
    
    first = 0
    last = nums.length - 1
    result = [-1, -1]
    
    while first + 1 < last
        mid = first + (last - first)/2
        first = mid if nums[mid] < target
        last = mid if nums[mid] >= target
    end
    
    if nums[first] == target
        result[0] = first 
    elsif nums[last] == target
        result[0] = last 
    else
        return [-1, -1]
    end
        
    first = 0
    last = nums.length - 1
        
    while first + 1 < last
        mid = first + (last - first)/2
        first = mid if nums[mid] <= target
        last = mid if nums[mid] > target
    end
        
    if nums[last] == target
        result[1] = last 
    elsif nums[first] == target
        result[1] = first 
    end
        
    return result
end
```

### Search Insert Position

```ruby
def search_insert(nums, target)
    return -1 if nums.empty?
    
    first = 0
    last = nums.length - 1
    
    while first + 1 < last
        mid = first + (last - first)/2    
        return mid if nums[mid] == target
        last = mid if nums[mid] > target
        first = mid if nums[mid] < target
    end
    
    return first if nums[first] >= target
    return last if nums[last] >= target
    
    return last + 1
end
```

