# 算法记录Day 9|Binary Tree

全面的关于binary tree的知识：https://www.geeksforgeeks.org/introduction-to-binary-tree-data-structure-and-algorithm-tutorials/?ref=lbp

需要掌握的（二刷继续）

### **Basic Operations On Binary Tree:**

- Inserting an element.
- Removing an element.
- Searching for an element.
- Deletion for an element.
- Traversing an element. There are four (mainly three) types of traversals in a binary tree which will be discussed ahead.

### **Auxiliary Operations On Binary Tree:**

- Finding the height of the tree

- Find the level of the tree

- Finding the size of the entire tree.

  

### **Binary Tree Traversals:**

Tree Traversal algorithms can be classified broadly into two categories:
Learn more

- Depth-First Search (DFS) Algorithms
- Breadth-First Search (BFS) Algorithms


## 二叉树的遍历之递归

递归算法的三个要素

（1）确定递归函数的**参数和返回值**： 确定哪些参数是递归的过程中需要处理的，那么就在递归函数里加上这个参数， 并且还要明确每次递归的返回值是什么进而确定递归函数的返回类型。

（2）确定**终止条件**： 写完了递归算法, 运行的时候，经常会遇到栈溢出的错误，就是没写终止条件或者终止条件写的不对，操作系统也是用一个栈的结构来保存每一层递归的信息，如果递归没有终止，操作系统的内存栈必然就会溢出。

（3）确定**单层递归的逻辑**： 确定每一层递归需要处理的信息。在这里也就会重复调用自己来实现递归的过程。


首先是二叉树的前序遍历：
参数： root, which is assumed to be an instance of the TreeNode class. 返回值：list

终止条件：当数为空时（包括了两种情况，1）当树为空 2）当为叶子节点（即子树为空）

单层递归的逻辑：如果子树非空，记录根节点，然后
```
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        if not root:
            return []

        left = self.preorderTraversal(root.left)
        right = self.preorderTraversal(root.right)

        return  [root.val] + left +  right
```

go through the steps of the preorderTraversal function for this example:
```
    1
   / \
  2   3
 / \
4   5
```

Initial call: root is the root of the entire tree (with value 1).

`[root.val]` is responsible for recording the value of the current node (root). So, when we make the initial call to preorderTraversal(root) with root being the root of the entire tree (with value 1),
[root.val] is [1], and this value gets recorded in the final result.

Since root is not None, we proceed to the next step.
Recursive calls:

We make a recursive call on the left child of the current node (root.left).
`root.left` is a node with value `2`.

Recursive call on `2`: root is 2 (not None), so we proceed to the next step.

Recursive call on the left child of 2 (`root.left`), which is a node with value `4`.

Recursive call on 4: `root` is 4 (not None), so we proceed to the next step.

Recursive call on the left child of 4 (`root.left`), which is None. Returns an empty list ([]).

Recursive call on the right child of 4 (`root.right`), which is None. Returns an empty list ([]).

Concatenate [4] + [] + [], resulting in [4].

Recursive call on the right child of 2 (`root.right`), which is a node with value `5`.

Recursive call on 5: root is 5 (not None), so we proceed to the next step.

Recursive call on the left child of 5 (`root.left`), which is `None`. Returns an empty list ([]).

Recursive call on the right child of 5 (`root.right`), which is `None`. Returns an empty list ([]).

Concatenate `[5] + [] + []`, resulting in `[5]`.

Concatenate `[2] + [4] + [5]`, resulting in `[2, 4, 5]`.

We make a recursive call on the right child of the current node (root.right).root.right is a node with value `3`.

Recursive call on 3: `root` is 3 (not None), so we proceed to the next step.

Recursive call on the left child of 3 (`root.left`), which is None. Returns an empty list ([]).

Recursive call on the right child of 3 (`root.right`), which is None. Returns an empty list ([]).

Concatenate `[3] + [] + []`, resulting in `[3]`.

Concatenate `[1] + [2, 4, 5] + [3]`, resulting in the final result `[1, 2, 4, 5, 3]`.

The final result is `[1, 2, 4, 5, 3]`, which is the preorder traversal of the given binary tree.

#### Create instances of the TreeNode class to represent this tree:
```
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

# Create the tree
root = TreeNode(1)
root.left = TreeNode(2)
root.right = TreeNode(3)
root.left.left = TreeNode(4)
root.left.right = TreeNode(5)

# call preorderTraversal function to traverse this tree:
solution = Solution()
result = solution.preorderTraversal(root)
print(result)
```
