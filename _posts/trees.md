# 二叉树的遍历：

目录
- 生成树
- DFS（三种顺序，前中后序）
    - 递归
    - 迭代
    - 迭代统一写法
- BFS
- 回溯


## 生成树
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

## 遍历
- 深度优先遍历：先往深走，遇到叶子节点再往回走。要义是一个方向去搜，不到黄河不回头，直到遇到绝境了，搜不下去了，再换方向（换方向的过程就涉及到了回溯）。用递归的方式来实现是最方便的。
- 广度优先遍历：一层一层的去遍历。把本节点所连接的所有节点遍历一遍，走到下一个节点的时候，再把连接节点的所有节点遍历一遍，搜索方向更像是广度，四面八方的搜索过程。

从深度优先遍历和广度优先遍历进一步拓展，才有如下遍历方式：

**深度优先遍历DFS的**
  - 前序遍历（递归法，迭代法）- 中左右
  - 中序遍历（递归法，迭代法）- 左中右
  - 后序遍历（递归法，迭代法）- 左右中

![image](https://github.com/aprilmingyli/aprilmingyli.github/assets/148889738/088566ed-8824-4497-b92f-7dcabe241038)

前中后序指的就是**中间节点**的位置，中间节点的顺序就是所谓的遍历方式

### 二叉树的递归遍历
递归Recursion三要素：

- 确定递归函数的*参数*和*返回值*： 确定哪些参数是递归的过程中需要处理的，那么就在递归函数里加上这个参数， 并且还要明确每次递归的返回值是什么进而确定递归函数的返回类型。

- 确定*终止条件*： 写完了递归算法, 运行的时候，经常会遇到栈溢出的错误，就是没写终止条件或者终止条件写的不对，操作系统也是用一个栈的结构来保存每一层递归的信息，如果递归没有终止，操作系统的内存栈必然就会溢出。

- 确定*单层递归的逻辑*： 确定每一层递归需要处理的信息。在这里也就会重复调用自己来实现递归的过程。

Leetcode 144 preorder traversal example:
```py
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
-----------------------------------------
**前序遍历（迭代法）**
前序遍历是中左右，处理顺序: 中间（根）节点 -> 右孩子 ->左孩子。

为什么要先加入右孩子，再加入左孩子呢？ 因为这样出栈的时候才是中左右的顺序。这样也保证上一轮的左孩子（栈顶）会是下一个中结点（current node），满足中左右的顺序。

在使用迭代法写中序遍历，需要记录**待处理**的节点，同时也需要记录**已经遍历过**的节点的值。
- stack 用来暂存待处理的节点。
它的作用类似于递归中的函数调用栈，通过栈的先进后出特性，可以实现树的前序遍历。在遍历过程中，不断将需要处理的节点加入 stack 中，然后从栈中取出节点进行处理，直到栈为空为止。
- result 则用来存储遍历过程中经过的节点的值，从而得到最终的前序遍历结果。
  
```py
def preorderTraversal(self, root: TreeNode) -> List[int]:
  if not root:
    return []
    
  stack = [root]
  result = []
  while stack:
    # 确定node指针指向current node
    node = stack.pop()
    # 先把中结点加入result中
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

---------------------------------------------------------------
**中序遍历**
一路向左走，途径的node都进栈。直到走到最深处，左孩子为空时，节点出栈，进入result。
cur指针指向已出栈的节点的右孩子。至此已经完成了最末端结点的左中右遍历。右孩子也为空，说明左子树处理完毕，父节点加入result
```py
  if not root:
    return []
    
  stack = []
  result = []
  cur = root
  while cur or stack:
    if cur:
      stack.append(cur)
      cur = cur.left
    else:
      cur = stack.pop()
      result.append(cur.val)
      cur = cur.right
  return result

```
------------------------------
**后序遍历**

后序遍历的结果是左右中。进栈左右，出栈右左，加上中，result是中右左。翻转即使左右中。

```py
# 后序遍历-迭代-LC145_二叉树的后序遍历
class Solution:
   def postorderTraversal(self, root: TreeNode) -> List[int]:
       if not root:
           return []
       stack = [root]
       result = []
       while stack:
           node = stack.pop()
           # 中结点先处理
           result.append(node.val)
           # 左孩子先入栈
           if node.left:
               stack.append(node.left)
           # 右孩子后入栈
           if node.right:
               stack.append(node.right)
       # 将最终的数组翻转
       return result[::-1]
```

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

**中序遍历**
```py
def preorderTraversal(root):
    result = []
    st = []
    if root:
        st.append(root)
    while st:
        node = st.pop()
        if node != None:
            print(f"st remove the top:{[n.val if n else None for n in st]}")
            if node.right: # 右
                st.append(node.right)
            st.append(node)  # 中
            st.append(None)
            if node.left: # 左
                st.append(node.left)
            print(f"st at the end is:{[n.val if n else None for n in st]}")
        else:
            node = st.pop()
            result.append(node.val)
            print(f"st in else:{[n.val if n else None for n in st]}")  # 打印 st 列表的内容
            print(f"result:{result}")
            print('-------------------')

    return result
```

结果print out：可以看出当一个中结点被遍历后就会被加入st，之后再用None作为一个flag，标志None之前的结点已经被处理过了，顺序已经被定了。
而None之后的结点，会经历弹出再压回的操作（比如4），弹出是为了再压回时可以保持正确的遍历顺序。
```py
st remove the top:[]
st at the end is:[6, 5, None, 4]
st remove the top:[6, 5, None]
st at the end is:[6, 5, None, 2, 4, None, 1]
st remove the top:[6, 5, None, 2, 4, None]
st at the end is:[6, 5, None, 2, 4, None, 1, None]
st in else:[6, 5, None, 2, 4, None]
result:[1]
-------------------
st in else:[6, 5, None, 2]
result:[1, 4]
-------------------
st remove the top:[6, 5, None]
st at the end is:[6, 5, None, 2, None]
st in else:[6, 5, None]
result:[1, 4, 2]
-------------------
st in else:[6]
result:[1, 4, 2, 5]
-------------------
st remove the top:[]
st at the end is:[8, 6, None, 7]
st remove the top:[8, 6, None]
st at the end is:[8, 6, None, 7, None]
st in else:[8, 6, None]
result:[1, 4, 2, 5, 7]
-------------------
st in else:[8]
result:[1, 4, 2, 5, 7, 6]
-------------------
st remove the top:[]
st at the end is:[8, None]
st in else:[]
result:[1, 4, 2, 5, 7, 6, 8]
-------------------
[1, 4, 2, 5, 7, 6, 8]
```

**后序遍历**
```py
class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        result = []
        st = []
        if root:
            st.append(root)
        while st:
            node = st.pop()
            if node != None:
                st.append(node) #中
                st.append(None)
                
                if node.right: #右
                    st.append(node.right)
                if node.left: #左
                    st.append(node.left)
            else:
                node = st.pop()
                result.append(node.val)
        return result
```

### 二叉树的层序遍历 BFS

**广度优先遍历**
从左到右一层一层的去遍历二叉树需要借用一个辅助数据结构即**队列**来实现，队列先进先出，符合一层一层遍历的逻辑，而用栈先进后出适合模拟深度优先遍历也就是递归的逻辑（上节已讲）。

这个算法的巧妙之处在于上一次迭代中队列里有几个节点，这一次循环就会执行几次。因为在每次循环开始时，我们使用 range(len(queue)) 来循环处理当前层的节点。由于 len(queue) 就是上一次循环结束后队列中的节点数量，因此在每次循环中，我们会处理当前层的节点数量次，这样保证了一层的结点就会被放进一个level的list中 -- [[1], [2, 3], [4, 5]]分别是三个level。

```py
def levelOrder(root):
  if not root:
      return []
  queue = collections.deque([root])
  result = []
  while queue:
      level = []
      for _ in range(len(queue)):
          cur = queue.popleft()
          level.append(cur.val)
          if cur.left:
              queue.append(cur.left)
          if cur.right:
              queue.append(cur.right)
      result.append(level)
  return result  
```

以上代码的output
```py
queue is:[1]
queue at the end is:[2, 3]
result is : [[1]]
------------
上一个for循环结束，queue size = 2（[2, 3]), 此次for 循环的range是（0，2），level中的结果将是两个。
queue is:[2, 3]
# 2 pop出去，2的两个孩子进队列。
queue at the end is:[3, 4, 5]
# 3 即将被popleft，3的孩子进入队列。在这个tree里3没有孩子，所以队列仍是[3, 4, 5]
queue is:[3, 4, 5]
queue at the end is:[4, 5]
result is : [[1], [2, 3]]
------------
queue is:[4, 5]
queue at the end is:[5]
queue is:[5]
queue at the end is:[]
result is : [[1], [2, 3], [4, 5]]
------------
[[1], [2, 3], [4, 5]]
```


## 回溯（Backtracking）：

回溯是一种通过尝试所有可能的解决方案来解决问题的方法。它通常用于解决组合问题、排列问题、棋盘游戏等需要尝试多种选择的情况。
回溯算法会尝试一条路径直到无法继续，然后返回上一步尝试其他路径，以此类推，直到找到解决方案或者确定无解。
典型的回溯问题包括八皇后问题、0-1背包问题等。
递归（Recursion）：

递归是一种通过在函数内部调用自身来解决问题的方法。它将问题分解为更小的子问题，并通过不断地调用自身来解决这些子问题。
递归通常用于解决具有递归结构的问题，如树的遍历、阶乘计算、斐波那契数列等。
递归需要注意避免无限递归的情况，通常通过设置递归的终止条件来解决。
迭代（Iteration）：

迭代是一种通过循环重复执行某段代码来解决问题的方法。它通过不断重复执行相同的步骤来逐步逼近问题的解决方案。





