# Binary Search

## 二分查找模版

* 牢记四要素
  * `start + 1 < end` - 【结束条件】头尾指针相交结束，防止死循环
  * `mid = start + (end - start)/2` - 【加分项】考虑到溢出
  * `nums[mid] == < >`, `start = mid`, `end = mid` - 【指针变化】不会漏解
  * `nums[start]`, `nums[end]`, `target` - 【结果选取】由first, last, any灵活选取
* 注意末尾结果选取，一旦第一个符合条件需立刻返回，避免结果改写
* 画图转化难题
* Rotated的问题，判断红线绿线是关键

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

```python
class Solution(object):
    def searchMatrix(self, matrix, target):
        if not matrix or not matrix[0]: return False

        m = len(matrix) - 1
        n = 0

        while m >= 0 and n <= len(matrix[0]) - 1:
            if matrix[m][n] == target: return True
            if matrix[m][n] < target:
                n += 1
            else:
                m -= 1

        return False
```

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

```python
class Solution(object):
    def firstBadVersion(self, n):
        first = 1
        last = n
        
        while first + 1 < last:
            mid = first + int((last - first)/2)
            if isBadVersion(mid) is True:
                last = mid
            else:
                first = mid
                
        if isBadVersion(first): return first
        if isBadVersion(last): return last
```

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

### Find Peak Element {#find-peak-element}

思路：上坡，下坡，谁大返回谁。

```ruby
def find_peak_element(nums)
    return -1 if nums.empty?
    return (nums.first >= nums.last ? 0 : 1) if nums.length < 3
    
    first = 0
    last = nums.length - 1
    
    while first + 1 < last
        mid = first + (last - first)/2
        if nums[mid] < nums[mid - 1]
            last = mid
        elsif nums[mid] < nums[mid + 1]
            first = mid
        else
            return mid
        end
    end
    
    return first if nums[first] > nums[last]
    return last if nums[last] > nums[first]
end
```

### Search in Rotated Sorted Array

思路：画图，将计就计，迎刃而解

```python
class Solution:
    def search(self, nums, target):
        if not nums: return -1
        
        first = 0
        last = len(nums) - 1
        
        while first + 1 < last:
            mid = first + int((last - first)/2)
            if nums[mid] == target: return mid
            if nums[0] < nums[mid]:
                if nums[0] <= target and target <= nums[mid]:
                    last = mid
                else:
                    first = mid
            else:
                if nums[mid] <= target and target <= nums[-1]:
                    first = mid
                else:
                    last = mid
                    

        if nums[first] == target: return first
        if nums[last] == target: return last
        return -1
```

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

### Find Minimum in Rotated Sorted Array {#find-minimum-in-rotated-sorted-array}

思路：画图，可能是纯递增，可能是正常 - 演变成找断点。

思路2：找第一个小于末项的数。

```python
class Solution:
    def findMin(self, nums):
        if not nums: return -1
        if nums[0] < nums[-1]: return nums[0]
        
        first = 0
        last = len(nums) - 1
        
        while first + 1 < last:
            mid = first + int((last - first)/2)
            if nums[mid] < nums[0]:
                last = mid
            else:
                first = mid

        return min(nums[first], nums[last])
```

```ruby
def find_min(nums)
    return nums.first if nums.first < nums.last
    first = 0
    last = nums.length - 1
    
    while first + 1 < last
        mid = first + (last - first)/2
        if nums.first <= nums[mid]
            first = mid
        else
            last = mid
        end
    end
    
    return nums[first] if nums[first] <= nums[last]
    return nums[last] if nums[last] < nums[first]
end
```

### Merge Sorted Array

```python
class Solution:
    def merge(self, nums1, m, nums2, n):
        curr = len(nums1) - 1
        m -= 1
        n -= 1
        while m >= 0 and n >= 0:
            if nums1[m] > nums2[n]:
                nums1[curr] = nums1[m]
                m -= 1
            else:
                nums1[curr] = nums2[n]
                n -= 1
            curr -= 1

        if m >= 0: nums1[:m + 1] = nums1[:m + 1]
        if n >= 0: nums1[:n + 1] = nums2[:n + 1]
```

