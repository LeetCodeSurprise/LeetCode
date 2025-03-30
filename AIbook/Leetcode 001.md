好的，我们来一起解决 LeetCode 第一题：两数之和。

### 题目描述 (LeetCode 001: Two Sum)

给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** `target` 的那 **两个** 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

**示例 1:**

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

**示例 2:**

```
输入：nums = [3,2,4], target = 6
输出：[1,2]
```

**示例 3:**

```
输入：nums = [3,3], target = 6
输出：[0,1]
```

### 解题思路 (从易到难)

1.  **暴力枚举 (Brute Force):**

    *   最简单的思路就是遍历所有可能的数字对，检查它们的和是否等于目标值。
    *   **时间复杂度：O(n^2)**  (两层循环)
    *   **空间复杂度：O(1)**  (没有使用额外空间)

2.  **哈希表 (Hash Map / Dictionary):**

    *   我们可以使用哈希表来存储已经遍历过的数字及其索引。
    *   对于每个数字 `nums[i]`，我们检查 `target - nums[i]` 是否存在于哈希表中。
    *   如果存在，说明找到了符合条件的两个数，返回它们的索引。
    *   如果不存在，将 `nums[i]` 及其索引存入哈希表。
    *   **时间复杂度：O(n)**  (平均情况下，哈希表查找是 O(1))
    *   **空间复杂度：O(n)**  (最坏情况下，哈希表需要存储所有数字)

### Python 代码实现

```python
# 暴力枚举 (Brute Force)
def twoSum_bruteForce(nums, target):
    n = len(nums)
    for i in range(n):
        for j in range(i + 1, n):
            if nums[i] + nums[j] == target:
                return [i, j]
    return []  # 如果没有找到，返回空列表

# 哈希表 (Hash Map / Dictionary)
def twoSum_hashTable(nums, target):
    num_map = {}  # 创建一个空字典来存储数字和索引
    for i, num in enumerate(nums):
        complement = target - num
        if complement in num_map:
            return [num_map[complement], i]
        num_map[num] = i  # 将当前数字和索引存入字典
    return []  # 如果没有找到，返回空列表
```

**代码解释：**

*   `twoSum_bruteForce`: 实现了暴力枚举方法，两个嵌套循环遍历所有数字对，查找和为 `target` 的组合。
*   `twoSum_hashTable`: 实现了哈希表方法。`num_map` 字典存储已经遍历过的数字和它们的索引。  对于每个数字，我们计算它的 `complement` （即 `target - num`），然后在 `num_map` 中查找 `complement` 是否存在。 如果存在，说明我们找到了两个数的和为 `target`，返回它们的索引。

**测试用例：**

```python
nums = [2, 7, 11, 15]
target = 9
print(f"暴力枚举结果: {twoSum_bruteForce(nums, target)}")  # 输出: 暴力枚举结果: [0, 1]
print(f"哈希表结果: {twoSum_hashTable(nums, target)}")  # 输出: 哈希表结果: [0, 1]

nums = [3, 2, 4]
target = 6
print(f"暴力枚举结果: {twoSum_bruteForce(nums, target)}")  # 输出: 暴力枚举结果: [1, 2]
print(f"哈希表结果: {twoSum_hashTable(nums, target)}")  # 输出: 哈希表结果: [1, 2]

nums = [3, 3]
target = 6
print(f"暴力枚举结果: {twoSum_bruteForce(nums, target)}")  # 输出: 暴力枚举结果: [0, 1]
print(f"哈希表结果: {twoSum_hashTable(nums, target)}")  # 输出: 哈希表结果: [0, 1]
```

### 要学习的要点

*   **时间复杂度分析:** 了解不同算法的时间复杂度，能够评估算法的效率。
*   **空间复杂度分析:** 了解算法所需的额外空间。
*   **哈希表 (Hash Table / Dictionary):** 掌握哈希表的基本概念和使用，包括查找、插入、删除等操作。哈希表在解决很多算法问题时非常有用。
*   **枚举 (Brute Force):** 了解最简单直接的解法，虽然效率不高，但可以作为对比和参考。
*   **Python 语法:**
    *   列表 (List) 的使用。
    *   字典 (Dictionary) 的使用。
    *   `enumerate()` 函数 (可以同时获取索引和值)。
    *   `in` 关键字 (用于判断元素是否存在于列表中或字典的键中)。

### 相关的 LeetCode 其他题目

*   **167. 两数之和 II - 输入有序数组:**  (可以使用双指针优化)
    [https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/)
*   **15. 三数之和:** (可以使用排序 + 双指针)
    [https://leetcode.cn/problems/3sum/](https://leetcode.cn/problems/3sum/)
*   **18. 四数之和:** (可以使用排序 + 双指针)
     [https://leetcode.cn/problems/4sum/](https://leetcode.cn/problems/4sum/)
*   **219. 存在重复元素 II:** (使用哈希表)
    [https://leetcode.cn/problems/contains-duplicate-ii/](https://leetcode.cn/problems/contains-duplicate-ii/)

通过练习这些题目，你可以更好地掌握哈希表的使用以及其他常用的算法技巧。 记住，学习算法是一个循序渐进的过程，从简单到复杂，不断练习才能提高。
