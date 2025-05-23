** AC 自动机 **

**核心步骤 (Core Steps):**

**核心疑惑**： 为什么 “在构建失败指针时，**必须**将 `fail` 指针指向节点的结束信息合并到当前节点的结束信息中？”

**答案**： 因为在匹配过程中，如果跳到fail指向的节点，会直接进入到fail指向节点的下一个节点，不会处理fail指向的节点。所以要讲`fail` 指针指向节点的结束信息合并到当前节点的结束信息。

AC 自动机的构建和使用主要分为以下三个核心步骤：

1.  **构建 Trie (字典树 / Keyword Tree):**
    *   **目的:** 将所有待查找的模式串（关键词）高效地存储起来，共享相同的前缀。
    *   **过程:**
        *   创建一个根节点。
        *   遍历每一个模式串。
        *   对于模式串中的每个字符，从当前节点出发，沿着对应的字符边向下移动。如果边不存在，则创建新的节点和边。
        *   在每个模式串结束时到达的节点上，标记该模式串的结束（例如，存储模式串的索引或标识到一个列表中）。

2.  **构建失败指针 (`fail` Pointers):**
    *   **目的:** 为 Trie 中的每个节点 `u` 计算一个 `fail` 指针，指向代表 `u` 对应字符串的“最长可识别后缀”的节点 `v`。这个后缀 `v` 本身也必须是 Trie 中的一个节点（即某个模式串的前缀）。这是为了在文本匹配失配时，能够快速跳转到下一个最有希望匹配上的状态，避免回溯文本。
    *   **过程 (使用 BFS):**
        *   初始化队列，将根节点的所有直接子节点入队，并将它们的 `fail` 指针指向根节点。
        *   当队列不为空时，出队一个节点 `u`。
        *   遍历 `u` 的每个字符 `c` 对应的子节点 `v`。
        *   **为 `v` 查找 `fail` 目标:** 从 `u` 的 `fail` 指针 (`u.fail`) 开始，沿着 `fail` 链向上（`temp = temp.fail`）查找，直到找到一个节点 `temp` 拥有字符 `c` 的子节点，或者到达根节点。
        *   **设置 `v` 的 `fail` 指针:**
            *   如果找到了这样的 `temp` 并且它有 `c` 子节点 `w`，则 `v.fail = w`。
            *   否则（最终到达根节点，或根节点也没有 `c` 子节点），`v.fail = root`。
        *   **关键 - 合并输出信息:** 将 `v.fail` 指向的节点的“结束标记”（即哪些模式串在此结束）信息 **合并** 到节点 `v` 的“结束标记”列表中。这确保了在匹配时访问一个节点就能知道所有以该节点对应字符串为后缀的模式串。

3.  **匹配文本 (Text Matching):**
    *   **目的:** 使用构建好的 AC 自动机在目标文本串中查找所有模式串的出现。
    *   **过程:**
        *   初始化当前状态 `currentNode = root`。
        *   遍历文本串中的每个字符 `c` (索引为 `i`)。
        *   **状态转移 (处理失配):** 如果 `currentNode` 没有字符 `c` 的子节点，并且 `currentNode` 不是根节点，则沿着 `fail` 指针移动 (`currentNode = currentNode.fail`)，重复此步骤直到找到有 `c` 子节点的节点或回到根节点。
        *   **前进:** 如果（经过 `fail` 跳转后）`currentNode` 有字符 `c` 的子节点 `nextNode`，则更新 `currentNode = nextNode`。否则，`currentNode` 保持为 `root`（或变为 `root`）。
        *   **检查并记录匹配:** 访问**当前** `currentNode` 的“结束标记”列表。对于列表中的每个模式串标识，记录一次匹配（例如，记录模式串标识和它在文本中的结束位置 `i`）。由于步骤 2 中的信息合并，这一步就能找到所有以当前位置 `i` 结尾的模式串匹配。

**注意事项 (Considerations):**

1.  **失败指针的构建顺序:** 必须使用 BFS（广度优先搜索）来构建失败指针，确保在计算节点 `v` 的 `fail` 指针时，其父节点 `u` 以及 `u.fail` (及 `fail` 链上的节点) 的 `fail` 指针已经被正确计算。
2.  **合并输出信息:** 在构建失败指针时，**必须**将 `fail` 指针指向节点的结束信息合并到当前节点的结束信息中。这是确保 AC 自动机能够找出所有匹配（包括是一个匹配后缀的较短模式串）的关键。如果省略这一步，将只能找到那些精确匹配到 Trie 叶子节点的模式串。
3.  **根节点的 `fail` 指针:** 根节点的 `fail` 指针通常指向自身，这可以简化边界条件的处理。
4.  **字符集:** 实现时要考虑字符集的大小。对于小写字母，可以使用固定大小的数组 `children[26]`；对于更大的字符集（如 ASCII、Unicode），应使用字典（如 Python 的 `dict` 或 C++ 的 `std::map`）来存储子节点。
5.  **大小写敏感:** AC 自动机默认是大小写敏感的。如果需要不区分大小写的匹配，应在构建 Trie 和匹配文本之前，将所有模式串和文本统一转换为相同的大小写。
6.  **匹配结果的处理:**
    *   标准 AC 自动机找出所有匹配，包括重叠的匹配（如在 "ushers" 中找到 "she" 和 "he" 都结束在索引 3）。
    *   如果只需要判断是否存在匹配，可以在找到第一个匹配后停止。
    *   如果需要非重叠匹配，需要在找到匹配后进行额外处理（例如，记录匹配区间，然后贪心选择或进行区间合并）。
    *   存储匹配结果（模式串索引、文本位置）可能需要大量内存，如果匹配数量巨大，考虑流式处理。
7.  **内存占用:** 当模式串数量巨大或总长度很长时，Trie 树本身可能消耗较多内存。
8.  **空模式串/空文本:** 算法通常不处理空模式串。对空文本的处理是自然的（循环不执行，无匹配）。
9.  **代码实现细节:** 注意指针（或引用）的操作，避免死循环，正确处理边界条件（如根节点）。

理解并正确实现这三个核心步骤，特别是失败指针的构建和信息合并，是掌握 AC 自动机的关键。注意这些事项能帮助你编写出正确且健壮的 AC 自动机实现。

```python

class Node:
    def __init__(self):
        self.children = {}
        self.fail = None
        self.out = []
    
class ACAutomation:
    def __init__(self,patterns):
        self.root = Node()
        self.patterns = patterns
    

    def _build_trie(self):

        for word in self.patterns:
            node = self.root
            for char in word:
                if char not in node.children:
                    node.children[char]=Node()
                node = node.children[char]
            # 注意，这里一定是append,有可能有重复
            node.out.append(word)
    
    def _build_failure_link(self):
        queue = []

        for node in self.root.children.values():
            queue.append(node)
            node.fail = self.root
        
        while queue:
            node = queue.pop()

            cur_fail = node.fail
            
            for char, char_node in node.items():

                # 注意这里不能忘记入队
                queue.append(char_node)

                while cur_fail is not None and char not in cur_fail.children:
                    cur_fail = cur_fail.fail
                
                # while结束条件1, 因为root.fail is None 我们没有给root的fail赋值
                # 当fail指向root，却root没有当前字符时，cur_fail 指向root.fail, 也就是None
                if cur_fail is None:
                    char_node.fail = self.root
                # while 结束条件 2： char in cur_fail.children
                else:
                    char_node.fail = cur_fail.children[char]
                
                # 这里要合并他的fail指向的输出，没有则为空
                char_node.out.extend(char_node.fail.out)

    def search(self,text):
        results = []

        cur_node = self.root

        for i in range(len(text)):
            char = text[i]

            while cur_node is not None and char not in cur_node.children:
                cur_node = cur_node.fail
            
            # while 结束条件1, cur_node 指向了root.fail, 
            # root 下面没有对应的字符，跳过
            if cur_node is None:
                cur_node = self.root
                continue
            
            # while 结束条件2 char 在cur_node.children
            cur_node = cur_node.children[char]

            # 检查结果
            if len(cur_node.out) > 0:
                for word in cur_node:
                    results.append((word,i-len(word)))
        return results
      
```
