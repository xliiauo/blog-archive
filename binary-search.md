# Binary Search

## 二分查找模版

* 牢记四要素
  * `start + 1 < end` - 【结束条件】头尾指针相交结束，防止死循环
  * `mid = start + (end - start)/2` - 【加分项】考虑到溢出
  * `nums[mid] == < >`, `start = mid`, `end = mid` - 【指针变化】不会漏解
  * `nums[start]`, `nums[end]`, `target` - 【结果选取】由first, last, any灵活选取
* 注意末尾结果选取，一旦第一个符合条件需立刻返回，避免结果改写
* 画图转化难题

### Binary Search - any

```python
def search(self, nums, target):
    if not nums:
        return -1

    first = 0
    last = len(nums) - 1

    while first + 1 < last:
        mid = first + int((last - first)/2)

        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            first = mid
        else:
            last = mid

    if nums[first] == target:
        return first
    elif nums[last] == target:
        return last
    else:
        return -1
```

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

### Search for a Range - first, last

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

思路：找第一个大于等于。

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

```python
class Solution(object):
    def searchInsert(self, nums, target):
        if not nums: return -1

        first = 0 
        last = len(nums) - 1
        
        while first + 1 < last:
            mid = first + int((last - first)/2)
            if nums[mid] >= target:
                last = mid
            else:
                first = mid

        if nums[first] >= target: return first
        if nums[last] >= target: return last
        
        return len(nums)
```

### Search a 2D Matrix

思路：列 - 最后一个小于等于，行 - 任意

```ruby
def search_matrix(matrix, target)
    return false if matrix.empty? || matrix.first.empty?
    return false if target.nil?
    
    first = 0
    last = matrix.size - 1
    
    while first + 1 < last
        mid = first + (last - first)/2
        return true if matrix[mid].first == target
        first = mid if matrix[mid].first < target
        last = mid if matrix[mid].first > target
    end
    
    if matrix[last].first == target || matrix[first].first == target
        return true
    elsif matrix[last].first < target
        m = last
    elsif matrix[first].first < target
        m = first
    else
        return false
    end
    
    nums = matrix[m]
    first = 0
    last = nums.size - 1
    
    while first + 1 < last
        mid = first + (last - first)/2
        return true if nums[mid] == target
        first = mid if nums[mid] < target
        last = mid if nums[mid] > target
    end
    
    return true if nums[first] == target
    return true if nums[last] == target
    
    return false
end
```

### Search a 2D Matrix II

思路：递增矩阵，固定打法

```ruby
def search_matrix(matrix, target)
  return false if matrix.empty? || matrix.first.empty? ||target.nil?
  m = matrix.length - 1
  n = 0

  while (m >=0 && n <= matrix.first.length - 1)
    return true if matrix[m][n] == target

    if matrix[m][n] > target
      m -= 1
    else
      n += 1
    end
  end

  return false
end
```

### First Bad Version

思路：first bad version 和 last good version + 1 一样，谁简单选谁

```ruby
def first_bad_version(n)
    first = 1
    last = n
    
    while first + 1 < last
        mid = first + (last - first)/2
        if is_bad_version(mid)
            last = mid 
        else
            first = mid
        end
    end
    
    return first if is_bad_version(first)
    return last if is_bad_version(last)
end
```

### Search in Rotated Sorted Array

思路：画图，将计就计，迎刃而解

```ruby
def search(nums, target)
    return -1 if nums.empty? || target.nil?
    first = 0
    last = nums.length - 1
    
    while first + 1 < last
        mid = first + (last - first)/2
        return mid if nums[mid] == target
         
        if nums.first < nums[mid]
            if nums.first <= target && target <= nums[mid]
                last = mid
            else
                first = mid
            end 
        else
            if nums[mid] <= target && target <= nums.last
                first = mid
            else
                last = mid
            end
        end
    end
    
    
    return first if nums[first] == target
    return last if nums[last] == target
    return -1
end
```



