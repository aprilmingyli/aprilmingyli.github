# 算法记录Day 6|哈希表

基础知识：
哈希表是根据关键码的值而直接进行访问的数据结构。一般哈希表都是*用来快速判断一个元素是否出现集合里*。但是哈希法也是牺牲了空间换取了时间，因为我们要使用额外的数组，set或者是map来存放数据，才能实现快速的查找。

hash table实例：

例如要查询一个名字是否在这所学校里。

要枚举的话时间复杂度是O(n)，但如果使用哈希表的话， 只需要O(1)就可以做到。

我们只需要初始化把这所学校里学生的名字都存在哈希表里，在查询的时候通过索引直接就可以知道这位同学在不在这所学校里了。

将学生姓名映射到哈希表上就涉及到了hash function ，也就是哈希函数。


## 242.有效的字母异位词

解法的核心思想：

创建一个长度为 26 的整数数组 record，用于记录每个小写字母出现的次数，数组的索引表示字母的相对位置，即 a 的位置为 0，b 的位置为 1，以此类推。

遍历字符串 s，对 s 中出现的字母，在 record 数组中对应位置加一。

遍历字符串 t，对 t 中出现的字母，在 record 数组中对应位置减一。

最后，遍历 record 数组，如果所有元素都为 0，说明两个字符串是异位词，返回 True；否则，返回 False。

这种方法的时间复杂度是 O(n)，空间复杂度是 O(1)，也可以说是 O(26)，因为 record 数组的长度是常数，固定为26，不管输入字符串 s 和 t 有多长，record 数组的大小都保持不变，因为它只存储了小写字母的出现次数，而小写字母总共只有 26 个。

```
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        # 创建一个长度为 26 的数组 record，用于记录字母出现的次数
        record = [0] * 26
        # 遍历字符串 s，统计每个字母出现的次数
        for i in s:
            record[ord(i) - ord("a")] += 1
        # 遍历字符串 t，将对应字母的次数减去
        for i in t:
            record[ord(i) - ord("a")] -= 1
        # 遍历 record 数组，如果有元素不等于 0，说明字符串 s 和 t 不是异位词
        for i in range(26):
            if record[i] != 0:
                return False
        return True


```

## 349. 两个数组的交集
（版本一） 使用字典和集合
使用哈希表 table 存储一个数组中的所有元素，然后遍历另一个数组，检查每个元素是否在 table 中出现，如果出现就添加到结果集合 res 中。

这种方法避免了重复元素的问题，最终返回的是交集元素的列表。这种解法的时间复杂度为 O(m + n)，其中 m 和 n 分别为两个输入数组的长度。
```
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
    # 使用哈希表存储一个数组中的所有元素
        table = {}
        for num in nums1:
            table[num] = table.get(num, 0) + 1
        
        # 使用集合存储结果
        res = set()
        for num in nums2:
            if num in table:
                res.add(num)
                del table[num]
        
        return list(res)
```

## 202 快乐数
创建一个空集合 record用于存储已经出现过的中间结果。

使用一个while True，不断进行以下操作：

调用 get_sum 函数，该函数接受一个整数 n，并计算 n 中各个数字的平方和。结果将替代原来的 n。
- 新的 n 等于1，返回 True，这是快乐数的特性。
- 新的 n 不等于1，就检查它是否已经在 record 集合中出现过：

如果出现过，表示陷入了一个循环，这不是一个快乐数，因此返回 False。
如果没有出现过，将新的 n 添加到 record 集合中，继续迭代。
get_sum 函数的作用是计算一个整数 n 中各个数字的平方和。它通过循环和取余运算遍历 n 中的每一位数字，将每个数字的平方加到 new_num 中，最后返回 new_num。

总结：这个代码使用循环和集合来检测一个数是否是快乐数。它通过计算数字的平方和并检查中间结果是否重复来判断。如果最终结果是1，那么这个数是快乐数，否则不是。
```
class Solution:
    def isHappy(self, n: int) -> bool:        
        record = set()

        while True:
            n = self.get_sum(n)
            if n == 1:
                return True
            
            # 如果中间结果重复出现，说明陷入死循环了，该数不是快乐数
            if n in record:
                return False
            else:
                record.add(n)

    def get_sum(self,n: int) -> int: 
        new_num = 0
        while n:
            n, r = divmod(n, 10)
            new_num += r ** 2
        return new_num
```

## 1 两数之和 
创建一个空的字典 records，该字典将用于存储已经访问过的元素和它们的下标。

使用一个循环来遍历整数列表 nums，同时获取元素的索引和值。这是通过 enumerate(nums) 实现的。

在每次迭代中，首先检查是否存在一个数，其值等于 target 减去当前元素的值。这是通过 if target - value in records 来实现的。如果存在这样的数，说明已经找到了一对数字，其和等于目标值 target，并且可以直接返回这两个数字的下标。

如果在 records 字典中没有找到匹配的数对，就将当前元素的值作为键，当前元素的索引作为值，添加到 records 字典中，以便在后续迭代中进行查找。

如果循环完成后仍然没有找到匹配的数对，就返回一个空列表 []，表示没有找到符合条件的数对。
```
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        records = dict()

        for index, value in enumerate(nums):  
            if target - value in records:   # 遍历当前元素，并在map中寻找是否有匹配的key
                return [records[target- value], index]
            records[value] = index    # 如果没找到匹配对，就把访问过的元素和下标加入到map中
        return []
```
