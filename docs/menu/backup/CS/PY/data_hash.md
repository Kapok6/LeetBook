[TOC]

## 总结

## 分类练习

705 设计哈希集合 简单 （待做）
706 设计哈希映射 简单 （待做）
387 字符串中的第一个唯一字符 简单 （待做）
15 三数之和 中等
18 四数之和 （待做）
49 字母异位词分组 中等 （待做）
36 有效的数独 中等 （待做）
652 寻找重复的子树 中等 （待做） 692.前 K 个高频单词 （待做）

### 字母异位词分组 49 M

1. 题目：对单词列表中的词，进行分类，异位词，归为一组。
2. 思路：
   1. defaultdict：其中 key:value 为“排序后的单词：所有异位词”。
   2. 返回结果：`list(dict.values)`

```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        from collections import defaultdict
        if not strs:
            return []
        voc = defaultdict(list)
        for s in strs:
            cur = ''.join(sorted(s))
            voc[cur].append(s)
        return list(voc.values())
```
