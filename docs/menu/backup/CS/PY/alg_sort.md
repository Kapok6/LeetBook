[TOC]

## 总结

## 分类练习

- 922.按奇偶排序数组 II E 双指针（易错）
- 1030.距离顺序排列矩阵单元格 E
- 56.合并区间 M 双指针 （见双指针专题）
- 75.颜色分类 M （见双指针专题）（易错）
- 148.排序链表 M （待做）
- 33.搜索旋转排序数组 （中） （见二分专题）
- 面试题 51 数组中的逆序对 （难）
- 324 摆动排序 中等（待做）
- 162 寻找峰值 中等（待做）
- 287 寻找重复数 中等（待做）
- 315 计算右侧小于当前元素的个数 困难（待做）
- 179.最大数 中等 排序+字符串处理
- 1305.两棵二叉搜索树中的所有元素 中等 排序+二叉搜索树
- 853.车队 中等 排序+遍历
- 1333.餐厅过滤器 中等 排序

### 常见排序

```python
# 冒泡排序
def bubble_sort(s):
    if len(s) < 2:
        return s
    for i in range(0, len(s)):
        for j in range(0, len(s) - i - 1):
            if s[j] > s[j + 1]:
                s[j], s[j + 1] = s[j + 1], s[j]
    print(s)
```

```python
# 选择排序
def select_sort(s):
    mlen = len(s)
    for i in range(mlen):
        min_index, min_val = i, s[i]
        # 当前待插入值的下标，即无序区第一个位置
        index = i
        # 在无序区，找到最小值及其下标；找到后与无序区第一个元素交换
        for j in range(i+1, mlen):
            if s[j] < min_val:
                min_index, min_val = j, s[j]
        s[index], s[min_index] = s[min_index], s[index]
    print(s)
```

```python
# 插入排序
def insert_sort(s):
    for i in range(1,len(s)):
        mcur = s[i]
        # for(start,end,步长)，(i,-1,-1)到下标0结束，逆序遍历数组
        for j in range(i,-1,-1):
            # 若当前元素大于待插入元素，则当前元素后移
            if s[j-1]>mcur:
                s[j] = s[j-1]
            else:
                break
        s[j] = mcur
    print (s)
```

```python
# 归并排序
def merge_sort(arr):
    #合并两个有序数组arr[s:mid+1],arr[mid+1:e+1]
    def merge(arr,s,mid,e):
        i1, i2 = s, mid+1
        temp = []
        while i1<=mid and i2<=e:
            if arr[i1]<=arr[i2]:
                temp.append(arr[i1])
                i1 += 1
            else:
                temp.append(arr[i2])
                i2 += 1
        if i1<=mid:
            temp = temp+arr[i1:mid+1]
        if i2<=e:
            temp = temp+arr[i2:e+1]
        for i in range(len(temp)):
            arr[s+i]=temp[i]
    # 递归实现归并排序
    def sort(arr,s,e):
        if s == e: return
        mid = (s+e-1)//2
        sort(arr,s,mid)
        sort(arr,mid+1,e)
        merge(arr,s,mid,e)

    sort(arr,0,len(arr)-1)
    print(arr)
```

```python
def quickSort_sort(s):
    if len(s) < 2:
        return s
    std = s[0]  #第一个元素设为基准
    left = [x for x in s[1:] if x <= std]
    right = [x for x in s[1:] if x > std]
    return quickSort_sort(left) + [std] + quickSort_sort(right)
```

### 922.按奇偶排序数组 II E

1. 思路：妙，只看偶数位，若为奇数，则不断寻找奇数位上的偶数，进行交换
2. 代码

```python
class Solution:
    def sortArrayByParityII(self, A):
        if not A:return []
        j = 1
        mlen = len(A)
        for i in range(0,mlen,2):
            if A[i]%2:
                while A[j]%2:
                    j+=2
                A[i], A[j] = A[j], A[i]
        return A
```

### 面试题 51 数组中的逆序对 （难）

1. 思路 1：插入排序，移动次数=逆序对数（超时）
   - 逆序对数=移动次数=满有序度-有序度；
2. 解题思路 2：归并排序，在合并时，当右侧小于左侧时，更新逆序对数；

```python
def reversePairs(nums):
    cnt = 0
    n = len(nums)
    if n<=1:return 0
    for i in range(1,n):
        cur = nums[i]
        for j in range(i,-1,-1):
            if j>=1 and nums[j-1]>cur:
                nums[j] = nums[j-1]
                cnt+=1
            else:
                break
        nums[j]=cur
    return cnt
def reversePairs1(nums):
    cnt = [0]
    if len(nums)<=1:return 0
    def merge(nums,s,mid,e):
        l,r = s,mid+1
        temp = []
        while l<=mid and r<=e:
            if nums[l]<=nums[r]:
                temp.append(nums[l])
                l+=1
            else:
                temp.append(nums[r])
                r+=1
                cnt[0]+=mid-l+1
        if l<=mid:
            temp+=nums[l:mid+1]
        if r<=e:
            temp+=nums[r:e+1]
        for i in range(len(temp)):
            nums[s+i] = temp[i]

    def sort(nums,s,e):
        if s==e:return
        mid = (s+e-1)//2
        sort(nums,s,mid)
        sort(nums,mid+1,e)
        merge(nums,s,mid,e)
    sort(nums,0,len(nums)-1)
    return cnt[0]

```
