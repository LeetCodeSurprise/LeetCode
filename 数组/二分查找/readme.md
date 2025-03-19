# 二分查找 的整体思路

```python
def binarySearch(nums,target):
    # 先决条件，nums有序
    nums.sort()
    # 确定查找范围，建立左边界和右边界
    # 一定要注意区间的开闭问题，这里采用左闭右开，也就是
    # 查询区间包含nums[left]，但是不包含nums[right]
    left,right = 0,len(nums)
    while left < right:
        # 计算中间元素的位置 （要注意大数溢出问题）
        mid = left + (right-left)//2
        # 这里很重要，建立中间位置元素与目标值的关系
        if func(nums[mid],target):
            # 这种条件下，左边界移动，舍弃左边一半的区间
            left = mid + 1
        else:
            # 这种条件下，右边界移动，舍弃右边一半的区间
            right = mid
```
