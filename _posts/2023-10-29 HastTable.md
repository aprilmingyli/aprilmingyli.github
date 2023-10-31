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
