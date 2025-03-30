好的，我们来一起攻克 LeetCode 第 2 题：两数相加。  我会提供超级详细的解释，照顾到每一位算法新手。

### 题目描述 (LeetCode 002: Add Two Numbers)

给你两个 **非空** 的链表，表示两个非负的整数。它们每位数字都是按照 **逆序** 的方式存储的，并且每个节点只能存储 **一位** 数字。

请你将这两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。

**示例：**

```
输入：l1 = [2,4,3], l2 = [5,6,4]
输出：[7,0,8]
解释：342 + 465 = 807.
```

```
输入：l1 = [0], l2 = [0]
输出：[0]
```

```
输入：l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
输出：[8,9,9,9,0,0,0,1]
```

### 解题思路（面向初学者，超级详细）

这道题的难点在于：

*   **链表：** 如果你对链表不太熟悉，需要先了解链表的基本概念和操作。
*   **逆序存储：** 数字是逆序存储的，意味着链表的头节点是个位，第二个节点是十位，以此类推。
*   **进位：** 加法过程中可能会产生进位，需要正确处理。

我们从最简单的思路开始：

1.  **理解题意 (非常重要!)**

    *   想象一下，你在用纸笔进行加法运算。 这道题就是让你用代码模拟这个过程。
    *   **逆序存储：** 意味着你需要从链表的头节点开始相加，就像我们平时做加法从个位开始算一样。
    *   **链表：** 链表是由节点组成的，每个节点包含一个值（这道题里是数字）和一个指向下一个节点的指针。 最后一个节点的指针指向 `None`。

2.  **模拟手工加法 (核心思路)**

    *   **逐位相加：** 从两个链表的头节点开始，逐位相加。
    *   **处理进位：** 如果两个数字相加大于等于 10，就要产生进位。 把进位加到下一位的计算中。
    *   **构建新链表：** 将每位的计算结果（个位数）放到一个新的链表的节点中。

3.  **详细步骤 (配合图解更容易理解)**

    1.  **初始化：**

        *   `carry = 0`：进位，初始值为 0。
        *   `dummy_head = ListNode(0)`：创建一个哑节点作为新链表的头节点。 哑节点只是为了方便操作，最后返回结果时会去掉它。
        *   `current = dummy_head`：`current` 指针指向当前要添加节点的指针。

    2.  **循环遍历：**

        *   循环条件：只要 `l1` 或 `l2` 还有节点，或者 `carry` 不为 0，就继续循环。
        *   **取值：**
            *   如果 `l1` 已经遍历完了（`l1` 为 `None`），就认为它的值为 0。
            *   如果 `l2` 已经遍历完了（`l2` 为 `None`），就认为它的值为 0。
            *   `x = l1.val if l1 else 0`
            *   `y = l2.val if l2 else 0`
        *   **计算和：** `sum = x + y + carry`
        *   **计算进位：** `carry = sum // 10` (例如，如果 `sum` 是 15，那么 `carry` 就是 1)
        *   **计算个位数：** `digit = sum % 10` (例如，如果 `sum` 是 15，那么 `digit` 就是 5)
        *   **创建新节点：** `current.next = ListNode(digit)`  (创建一个新的节点，值为 `digit`，并把它添加到新链表的末尾)
        *   **移动指针：**
            *   `current = current.next`  (把 `current` 指针移动到新链表的下一个节点)
            *   如果 `l1` 还没有遍历完，就 `l1 = l1.next`，否则 `l1` 保持 `None`。
            *   如果 `l2` 还没有遍历完，就 `l2 = l2.next`，否则 `l2` 保持 `None`。

    3.  **返回结果：**

        *   `return dummy_head.next` (返回新链表的头节点，去掉哑节点)

4.  **举例说明：**

    假设 `l1 = [2, 4, 3]`，`l2 = [5, 6, 4]`

    *   **第一次循环：**
        *   `x = 2`, `y = 5`, `carry = 0`
        *   `sum = 2 + 5 + 0 = 7`
        *   `carry = 7 // 10 = 0`
        *   `digit = 7 % 10 = 7`
        *   新链表：`[0 -> 7]`
    *   **第二次循环：**
        *   `x = 4`, `y = 6`, `carry = 0`
        *   `sum = 4 + 6 + 0 = 10`
        *   `carry = 10 // 10 = 1`
        *   `digit = 10 % 10 = 0`
        *   新链表：`[0 -> 7 -> 0]`
    *   **第三次循环：**
        *   `x = 3`, `y = 4`, `carry = 1`
        *   `sum = 3 + 4 + 1 = 8`
        *   `carry = 8 // 10 = 0`
        *   `digit = 8 % 10 = 8`
        *   新链表：`[0 -> 7 -> 0 -> 8]`
    *   **循环结束：**
        *   返回 `[7 -> 0 -> 8]`

### Python 代码实现

首先，我们需要定义链表节点：

```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
```

然后是 `addTwoNumbers` 函数：

```python
def addTwoNumbers(l1, l2):
    carry = 0  # 初始化进位
    dummy_head = ListNode(0)  # 创建哑节点
    current = dummy_head  # current 指针指向当前节点

    while l1 or l2 or carry:  # 循环条件：l1 或 l2 还有节点，或者有进位
        x = l1.val if l1 else 0  # 取 l1 的值，如果 l1 为 None，则取 0
        y = l2.val if l2 else 0  # 取 l2 的值，如果 l2 为 None，则取 0
        sum = x + y + carry  # 计算和

        carry = sum // 10  # 计算进位
        digit = sum % 10  # 计算个位数

        current.next = ListNode(digit)  # 创建新节点
        current = current.next  # 移动 current 指针

        if l1:  # 移动 l1 指针
            l1 = l1.next
        if l2:  # 移动 l2 指针
            l2 = l2.next

    return dummy_head.next  # 返回结果 (去掉哑节点)
```

**代码解释：**

*   `ListNode`: 定义链表节点。
*   `addTwoNumbers`:  实现了两数相加的逻辑。  `carry` 变量跟踪进位。  `dummy_head` 用于简化新链表的构建。  循环遍历两个链表，逐位相加，处理进位，并将结果添加到新链表中。

**测试用例：**

```python
# 创建链表辅助函数
def create_linked_list(nums):
    dummy_head = ListNode(0)
    current = dummy_head
    for num in nums:
        current.next = ListNode(num)
        current = current.next
    return dummy_head.next

# 链表转列表辅助函数
def linked_list_to_list(head):
    result = []
    while head:
        result.append(head.val)
        head = head.next
    return result

# 测试用例
l1 = create_linked_list([2, 4, 3])
l2 = create_linked_list([5, 6, 4])
result = addTwoNumbers(l1, l2)
print(linked_list_to_list(result))  # 输出: [7, 0, 8]

l1 = create_linked_list([0])
l2 = create_linked_list([0])
result = addTwoNumbers(l1, l2)
print(linked_list_to_list(result)) # 输出: [0]

l1 = create_linked_list([9,9,9,9,9,9,9])
l2 = create_linked_list([9,9,9,9])
result = addTwoNumbers(l1, l2)
print(linked_list_to_list(result)) # 输出: [8, 9, 9, 9, 0, 0, 0, 1]
```

### 要学习的要点

*   **链表 (Linked List):**
    *   理解链表的基本概念：节点、指针、头节点、尾节点。
    *   链表的创建、遍历、插入、删除等操作。
*   **哑节点 (Dummy Node):** 学习如何使用哑节点简化链表操作。
*   **进位 (Carry):** 理解加法运算中的进位概念，并在代码中正确处理。
*   **取模和整除：**  `%` (取模运算符) 和 `//` (整除运算符) 的使用。
*   **指针操作：**  如何通过指针移动来遍历和修改链表。
*   **条件判断：** 如何处理 `l1` 和 `l2` 长度不同的情况。

### 相关的 LeetCode 其他题目

*   **206. 反转链表:**  (练习链表的基本操作)
    [https://leetcode.cn/problems/reverse-linked-list/](https://leetcode.cn/problems/reverse-linked-list/)
*   **83. 删除排序链表中的重复元素:** (练习链表的遍历和删除)
    [https://leetcode.cn/problems/remove-duplicates-from-sorted-list/](https://leetcode.cn/problems/remove-duplicates-from-sorted-list/)
*   **141. 环形链表:** (练习链表的快慢指针)
    [https://leetcode.cn/problems/linked-list-cycle/](https://leetcode.cn/problems/linked-list-cycle/)
*   **21. 合并两个有序链表:** (练习链表的合并)
    [https://leetcode.cn/problems/merge-two-sorted-lists/](https://leetcode.cn/problems/merge-two-sorted-lists/)

这道题的关键在于理解链表的结构和模拟手工加法的过程。 如果你对链表还不太熟悉，建议先做一些简单的链表题目，再来挑战这道题。 慢慢来，一步一个脚印，你一定能掌握！
