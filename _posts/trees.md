二叉树的遍历：

- 深度优先遍历：先往深走，遇到叶子节点再往回走。要义是一个方向去搜，不到黄河不回头，直到遇到绝境了，搜不下去了，再换方向（换方向的过程就涉及到了回溯）。用递归的方式来实现是最方便的。
- 广度优先遍历：一层一层的去遍历。把本节点所连接的所有节点遍历一遍，走到下一个节点的时候，再把连接节点的所有节点遍历一遍，搜索方向更像是广度，四面八方的搜索过程。

从深度优先遍历和广度优先遍历进一步拓展，才有如下遍历方式：

**深度优先遍历DFS**
  - 前序遍历（递归法，迭代法）- 中左右
  - 中序遍历（递归法，迭代法）- 左中右
  - 后序遍历（递归法，迭代法）- 左右中

![image](https://github.com/aprilmingyli/aprilmingyli.github/assets/148889738/088566ed-8824-4497-b92f-7dcabe241038)

前中后序指的就是**中间节点**的位置，中间节点的顺序就是所谓的遍历方式

**广度优先遍历**
  - 层次遍历（迭代法）一般使用队列来实现，这也是队列先进先出的特点所决定的，因为需要先进先出的结构，才能一层一层的来遍历二叉树。

生成树
```py
class TreeNode:
    def __init__(self, val, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

# 创建一棵二叉树
root = TreeNode(1)
root.left = TreeNode(2)
root.right = TreeNode(3)
root.left.left = TreeNode(4)
root.left.right = TreeNode(5)

# 打印这棵树的结构（中左右，中序遍历）
def print_tree(node, level=0):
    if node is not None:
        print('  ' * level + str(node.val))
        print_tree(node.left, level + 1)
        print_tree(node.right, level + 1)

print_tree(root)

##**result**##
1
  2
    4
    5
  3
```

### 二叉树的递归遍历
递归Recursion三要素：

- 确定递归函数的*参数*和*返回值*： 确定哪些参数是递归的过程中需要处理的，那么就在递归函数里加上这个参数， 并且还要明确每次递归的返回值是什么进而确定递归函数的返回类型。

- 确定*终止条件*： 写完了递归算法, 运行的时候，经常会遇到栈溢出的错误，就是没写终止条件或者终止条件写的不对，操作系统也是用一个栈的结构来保存每一层递归的信息，如果递归没有终止，操作系统的内存栈必然就会溢出。

- 确定*单层递归的逻辑*： 确定每一层递归需要处理的信息。在这里也就会重复调用自己来实现递归的过程。

Leetcode 144 preorder traversal example:
```
class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
      # step 1 确定参数和返回值。参数：tree root 返回值：完整的树
      # step 2 确定终止条件。终止条件：当遇到子节点为null
      # step 3 确定单层递归的逻辑。单层递归逻辑即递归的顺序，先左/右/中？
      if not root:
        return []

      left = self.preorderTraversal(root.left)
      right = self.preorderTraversal(root.right)

      # 前序遍历：中左右
      return [root.val] + left + right
```

### 二叉树的迭代遍历
前序遍历（迭代法）
前序遍历是中左右，处理顺序: 中间（根）节点 -> 右孩子 ->左孩子。

为什么要先加入 右孩子，再加入左孩子呢？ 因为这样出栈的时候才是中左右的顺序。


在使用迭代法写中序遍历，需要记录**待处理**的节点，同时也需要记录**已经遍历过**的节点的值，以便最终得到遍历结果。通过分别使用 stack 和 result
stack 是用来辅助进行遍历的栈，而result 是用来存储前序遍历结果的列表，互相配合。

stack 用来暂存待处理的节点。
它的作用类似于递归中的函数调用栈，通过栈的先进后出特性，可以实现树的前序遍历。在遍历过程中，不断将需要处理的节点加入 stack 中，然后从栈中取出节点进行处理，直到栈为空为止。
 
result 则用来存储遍历过程中经过的节点的值，从而得到最终的前序遍历结果。
```py
def preorderTraversal(self, root: TreeNode) -> List[int]:
  if not root:
    return []
    
  stack = [root]
  result = []
  while stack:
    # 确定node指针指向current node
    node = stack.pop()
    # 先处理中结点
    result.append(node.val)
    # 右孩子先入栈
    if node.right:
      stack.append(node.right)
    # 左孩子再入栈
    if node.left:
      stack.append(node.left)

  return result
```
 
前序遍历中*访问节点*（遍历节点）和*处理节点*（将元素放进result数组中）可以同步处理，但是中序就无法做到同步！所以代码风格无法做到统一。

### 二叉树迭代遍历之统一写法（用栈）
因为二叉树迭代遍历，在之前的小节里没有统一的写法，所以在此归纳出统一的写法：

```py
class TreeNode:
    def __init__(self, val, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def preorderTraversal(root):
    result = []
    st = []
    if root:
        st.append(root)
    while st:
        node = st.pop()
        print(f"st removed the top of the stack:{[n.val if n else None for n in st]}")
        if node != None:
            if node.right: # 右
                st.append(node.right)
            if node.left: # 左
                st.append(node.left)
            st.append(node)  # 中
            st.append(None)
            print(f"now st finished adding None:{[n.val if n else None for n in st]}")  # 打印 st 列表的内容
        else:
            node = st.pop()
            result.append(node.val)
            print(f"st in else:{[n.val if n else None for n in st]}")  # 打印 st 列表的内容
            print(f"result:{result}")
            print('-------------------')

    print(f"st at end:{[n.val if n else None for n in st]}")  # 打印 st 列表的内容
    return result

# 测试用例1：二叉树 [5,4,6,1,2,7,8]
root1 = TreeNode(5)
root1.right = TreeNode(6)
root1.right.right = TreeNode(8)
root1.right.left = TreeNode(7)
root1.left = TreeNode(4)
root1.left.right = TreeNode(2)
root1.left.left = TreeNode(1)

print(preorderTraversal(root1))
```
为了更直观地表现以上代码是如何运作的，把代码每一步的output打印出来

```py
st removed the top of the stack:[]
now st finished adding None:[6, 4, 5, None]
st removed the top of the stack:[6, 4, 5]
st in else:[6, 4]
result:[5]
-------------------
st removed the top of the stack:[6]
now st finished adding None:[6, 2, 1, 4, None]
st removed the top of the stack:[6, 2, 1, 4]
st in else:[6, 2, 1]
result:[5, 4]
-------------------
st removed the top of the stack:[6, 2]
now st finished adding None:[6, 2, 1, None]
st removed the top of the stack:[6, 2, 1]
st in else:[6, 2]
result:[5, 4, 1]
-------------------
st removed the top of the stack:[6]
now st finished adding None:[6, 2, None]
st removed the top of the stack:[6, 2]
st in else:[6]
result:[5, 4, 1, 2]
-------------------
st removed the top of the stack:[]
now st finished adding None:[8, 7, 6, None]
st removed the top of the stack:[8, 7, 6]
st in else:[8, 7]
result:[5, 4, 1, 2, 6]
-------------------
st removed the top of the stack:[8]
now st finished adding None:[8, 7, None]
st removed the top of the stack:[8, 7]
st in else:[8]
result:[5, 4, 1, 2, 6, 7]
-------------------
st removed the top of the stack:[]
now st finished adding None:[8, None]
st removed the top of the stack:[8]
st in else:[]
result:[5, 4, 1, 2, 6, 7, 8]
-------------------
st at end:[]
[5, 4, 1, 2, 6, 7, 8]
```
总结：除了root以外，上一个中结点处理完成以后，会被先pop出去，然后再被按照右左中的顺序加回来，这样出栈的时候就可以保证中左右的方式出去，满足前序遍历的需求。
每次遇到None之后，就知道中结点的左右孩子已经被加入st栈中，可按照顺序出栈了。




回溯（Backtracking）：

回溯是一种通过尝试所有可能的解决方案来解决问题的方法。它通常用于解决组合问题、排列问题、棋盘游戏等需要尝试多种选择的情况。
回溯算法会尝试一条路径直到无法继续，然后返回上一步尝试其他路径，以此类推，直到找到解决方案或者确定无解。
典型的回溯问题包括八皇后问题、0-1背包问题等。
递归（Recursion）：

递归是一种通过在函数内部调用自身来解决问题的方法。它将问题分解为更小的子问题，并通过不断地调用自身来解决这些子问题。
递归通常用于解决具有递归结构的问题，如树的遍历、阶乘计算、斐波那契数列等。
递归需要注意避免无限递归的情况，通常通过设置递归的终止条件来解决。
迭代（Iteration）：

迭代是一种通过循环重复执行某段代码来解决问题的方法。它通过不断重复执行相同的步骤来逐步逼近问题的解决方案。


### 二叉树的迭代遍历

### 二叉树迭代遍历之统一写法（用栈）
因为二叉树迭代遍历，在之前的小节里没有统一的写法，所以在此归纳出统一的写法：

相比与之前的迭代写法，我们多加一个`st`来对处理的结点进行flag 

当遇到一个节点时，我们将其入栈，同时入栈一个标志值 None，用来表示当前节点是中结点（先处理中结点）。然后，接着处理其右孩子和左孩子。

如果元素是 None，则表示这个节点已经处理过其左右子节点，需要将其值加入到结果中。
通过这种方式，我们在列表中记录了节点的处理状态，从而实现了前序遍历。


## 二叉树的层序遍历 BFS
迭代通常用于处理需要重复执行相似操作的问题，如数组遍历、矩阵运算等。
迭代可以通过循环结构实现，如for循环、while循环等。

回溯是一种特殊的搜索算法，通常用于解决涉及多个选择的问题，而递归和迭代更多地关注于解决单个问题的方法。
递归是一种通过函数调用自身来解决问题的方法，通常适用于处理递归结构的问题；而迭代则是通过循环重复执行某段代码来解决问题。
回溯和递归通常更加灵活，但可能会导致栈溢出等问题；迭代则更加直接，适用于处理简单且明确的重复操作。




