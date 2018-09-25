# Two Sum

HashMap - O\(n\), O\(n\)

```python
class Solution:
    def twoSum(self, nums, target):
        map = dict()
        for idx, val in enumerate(nums):
            if target - val in map:
                return [map[target - val], idx]
            else:
                map[val] = idx
```

Two Pointer/Brute Force - O\(n^2\), O\(1\)

```text
class Solution:
    def twoSum(self, nums, target):
        i = 0
        while i <= len(nums) - 1:
            j = i + 1
            while j <= len(nums) - 1:
                if nums[i] + nums[j] == target:
                    return [i, j]
                else:
                    j += 1
            i += 1  
```

