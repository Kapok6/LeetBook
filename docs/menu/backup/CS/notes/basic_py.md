## 1. 高频工具

### 1.1. 加速器

1. map：根据提供的函数对指定的序列做**映射**，函数常用lambda函数替代；`map(function, sequence...)`；
	```python
	# 将数组中的元素
	res = list(map(lambda x,y:x+y, [1, 2], [1, 2]))
	```

2. filter：用函数对指定序列做**过滤**，保留函数返回True的值；`filter(function, sequence)`;
	```python
	num = [1,2,3,4,5]
	res = list(filter(lambda x:x%2==1,num)) #[1,3,5]
	```

3. reduce：对参数序列中元素进行累积；
	1. `reduce(function, sequence)`,function必须是有2个参数的函数；
	2. py3中，需引入包：`from functools import reduce`
	```python
	from functools import reduce
	reduce(lambda x, y:x+y, [1,2,3,4]) # 返回10
	```
	
4. lambda：匿名函数,适合简单逻辑的函数；`lambda arg1,... : option`
	1. 可用于其他函数的参数，如`map,filter,reduce,sorted(arr,key=function)`；
	2. 也可赋值给一个变量，后面通过变量来调用该匿名函数；
	```python
	sorted([1, 2, 3, 4, 5, 6, 7, 8, 9], key=lambda x: abs(5-x))
	```

5. zip([iterable,...]):
	1. 将可迭代的对象作为参数，将对象中对应的元素打包成一个个元组，然后返回由这些元组组成的对象，这样做的好处是节约了不少的内存。
	2. 输出打包后的元祖：`list(zip(a,b,c))`
	3. 解压：`a1,b1,c1 = zip(*zip(a,b,c))`
	4. 注意：打包后元组的大小，以可迭代对象的最小规格为基础

	```python
		a,b,c=[1,2,3],[4,5],[6,7,8]
		# 压缩
    res = zip(a,b,c)
    print(list(res)) # [(1, 4, 6), (2, 5, 7)]
    # 解压
    a1,b1,c1 = zip(*zip(a,b,c))
    print(a1,b1,c1)  # (1, 2) (4, 5) (6, 7)
	```
### 1.2. collections库

1. 引入方式：`from collections import 包名`

2. 默认字典（defaultdict）：为字典中键值设置默认值
	
	输出结构，为`{'a': 6, 'b': 7}`
	
	```python
	dd = defaultdict(int) #值默认为0
	dd['key1']+=1 # 1
	
	dd2 = defaultdict(list) #值默认为[]
	dd2['key1'].append(3)#[3]
	
	dd3 = defaultdict(dict) #值默认为{}
	dd3['key1']['card']=122 #{'key1': {'card': 122}}
	
	dd4 = defaultdict(set) #值默认为（）
	dd4['key1'].add(3)#{'key1':(3)}
	```
	
3. 有序字典(OrderedDict)

   ```python
   # 直接生成“按输入有序”的字典
   d1 = collections.OrderedDict()
   d1['a'] = 'A'
   d1['c'] = 'C'
   d1['b'] = 'B'
   print(d1) #OrderedDict([('a', 'A'), ('c', 'C'), ('b', 'B')])
   print([i for i in d1.items()])#[('a', 'A'), ('c', 'C'), ('b', 'B')]，拿到list形式
   
   # 对已有字典(传统dict或defaultdict)，进行排序
   d1 = {'a':[3,4,2],'c':[1,5,6],'b':[8,2,1]}
   d2 = collections.OrderedDict(sorted(d1.items(),key=lambda x:x[0])) #按“key”排序
   d3 = collections.OrderedDict(sorted(d1.items(),key=lambda x:x[1][0]))#按“value”的第一个元素排序
   print(d2)#OrderedDict([('a', [3, 4, 2]), ('b', [8, 2, 1]), ('c', [1, 5, 6])])
   print(d3)#OrderedDict([('c', [1, 5, 6]), ('a', [3, 4, 2]), ('b', [8, 2, 1])])
   ```

4. 计数器（Counter）：提供了可哈希对象的计数功能,返回dict；
   
   传入值：字符串、字典、数组等；
   
   ```python
   from collections import Counter
   c = Counter(alist) #得到“数值：数量”的键值对
   c['x']+=1 #{x:1},计数
   voc = Counter('ababc')#({'a':2,'b':2,'c':1})
   #获取前k个元素：[(),()]
print(voc.most_common(2))
   #拿到字典形式
   mdict = dict(voc)
   #统计键/值
   mkeys,mvalues = voc.keys(),voc.values()
   c.update(alist) # 更新计数器
   # 删除某个键
   del c[key]
   ```
   
5. 双向队列（deque）
   ```python
   from collections import deque
   q = deque(['a', 'b', 'c'])
   q.append('x') # 尾部插入
   q.pop() # 尾部删除
   q.appendleft('y') # 头部插入/删除
   q.popleft() # 头部删除
   ```

### 1.3. bisect二分模块

**对有序序列的二分插入和查找**（避免超时）

1. 插入：

   - .insort和.insort_right一样，在右侧插入;
   - .insort_left：当待插入元素已存在，插入其左侧

   ```python
   import bisect
   test_list = [1, 2, 2, 4, 8]
   bisect.insort(test_list, 6)# [1,2,2,4,6,8]
   bisect.insort(test_list, 1)# [1, 1, 2,2,4,6,8]
   ```

2. 查找（元素插入的位置，不会插入）

   1. .bisect 和 .bisect_right一样
   2. .bisect_left

   ```python
   import bisect
   test_list = [1, 1, 2, 2, 4, 8, 15, 15]
   
   # 在相同元素的后面插入
   print(bisect.bisect(test_list, 2))#index=4(最后一个2后面)
   print(bisect.bisect(test_list, 6))#index=5(4后面)
   print(test_list)
   ```

### 1.4. 排列组合

1. permutations(iter,r)，返回iter中任意取r个元素做排列的迭代器;

2. combinations(iter,r),返回iter中任意取r个元素做排列的迭代器。组合不分顺序，比如ab, ba只返回一个ab;

3. 排列vs组合：排列，会出现所有元素相同但顺序不同的情况；组合不会；

   ```python
   from itertools import permutations
   n = 4
   k = 9
   a = [str(i) for i in range(1, n+1)]
   it = permutations(a, n)
   for i in range(5):
       print(next(it))
   ('1', '2', '3', '4')
   ('1', '2', '4', '3')
   ('1', '3', '2', '4')
   ('1', '3', '4', '2')
   ('1', '4', '2', '3')
   ```

### 1.5. 常用操作

#### 1.4.1. 排序

1. 方法：内置sort()无返回，sorted()有返回，默认升序，reverse=True降序；

2. 列表：

   ```python
   # 按元组的第3个元素值进行排序
   stu = [("lucy", "C", 16), ("john", "B", 14)]
   stu.sort(key=lambda x: x[2])#[('winnie', 'A', 12), ('john', 'B', 14)]
   
   # 按字典中的”age对应的值”进行排序
   l1 = [{'name0':'李丽', 'age': 40}, {'name0':'张那', 'age': 30}]
   l2 = sorted(l1, key=lambda x: x['age'])
   #[{'name0': '张那', 'age': 30}, {'name0': '李丽', 'age': 40}]
   ```

3. 字典：

   1. 使用sorted()方法：`sorted(mdict.items())`
   2. 使用list内置的sort方法：要**先将 字典》list**：`list(dict.items()).sort()`
   3. 使用zip封装(key,value)，再排序，`sorted(zip(dict.values(),dict.keys()))`
   
   ```python
   # 全升序: [('acd', 234), ('abc', 234), ('cd', 111)]
   mdict = {'acd':234,'cd':111,'abc':234}
   sorted(mdict.items(),key=lambda x:(x[1],x[0]))
   
   # 值降序、键升序：#[('abc', 234), ('acd', 234), ('cd', 111)]
   d2 = sorted(mdict.items()) 
   # 也可以考虑先读mdict排序，再对排序结果
   
   # 全降序:[('acd', 234), ('abc', 234), ('cd', 111)]
   sorted(mdict.items(),key=lambda x:(x[1],x[0]),reverse=True)
   
   # zip: [(111, 'cd'), (234, 'abc'), (234, 'acd')]
   sorted(zip(mdict.values(),mdict.keys()))
   ```

#### 1.4.3. 内置函数

```python
1. abs();
2. pow(x,y) #x^y;
3. round(x,numb) #x为整数/浮点数，numb几位小数，默认为0；（小数位不足，则不会进行补足）
4. eval(mstr) #运行字符串
5. bin(),oct(),hex():入，带进制符号的整数，出，带进制符号的字符串；
6. int()：`int(0xab)，int('ab',16),int('0xab',16)`，出，整数；
7. reverse(),reversed(),sort(),sorted()
8. zip([iterable,...])
9. '%.2f'%float(x)：将整数保留2位小数输出
10. ASCII码(整数)与字符：`ord('a'), chr(97)`
11. 进行非10进制之间的转换，需要借助10进制，将其转为整数，再换算；
```

#### 1.4.4. 正则

- `re.match(pat,str,flag)`：只从第一个开始匹配；匹配成功,可以使用group(num) 或 groups() 匹配对象函数来获取匹配表达式。匹配不成功，则返回None。
- `re.search(pat,str,flag)`：全局搜索；
- 起止：^,$，如 `'^a{1,2}b$'`；
- 获取匹配结果：`group,group(numb)`;
- `re.sub(pat,rep_str/rep_fun)`:`re.sub('\D','',mstr)`非数字则替换为空；

#### 1.4.5. 位运算

1. 与、或、非、异或：&，|，~，^不同为1，相同为0;

2. 位运算：
    1. 10进制数的位运算：10》2》运算》10
    2. 若想进行2进制的位运算：可考虑先》转10》运算》转2进制&输出

3. 判断一个二进制数中1的个数：
    1. 技巧：把一个整数减去1，再和原整数做与运算，会把该整数最右边一个1变成0；那么一个整数的二进制有多少个1，就可以进行多少次这样的操作。(这里的整数，是10进制数)
    2. 代码：
    ```
    def NumberOf1(n):
        if n < 0:
            # 与操作，为了得到： 负数（十进制表示）的补码
            n = n & 0xffffffff
        count = 0
        while n:
            n = n & (n - 1)
            count += 1
        return count
    ```
    3. 思路2：直接计算字符串中1的个数`res = sum([int(i) for i in bin_str[:2]])`

#### 1.4.6. Tips

1. 数组判空：`if not arr`
2. 数组排序：`intervals.sort(key=lambda x: x[0])`
3. 元素最后一次出现的下标：`last_index = {c:i for i,c in enumerate (S)}`
4. 数组遍历：`for i in range()`,`for i,v in enumerate()`
5. 数组遍历，设置步长：`for i in range(start,end,step)`
6. 求中位数：`mid=s+(e-s)//2`

### 1.5. 方法汇总

1. 增
	1. str：`insert(index,sub)`, 拼接`'sep'.join(str)`；
	2. list：`insert(index,sub);append()`,
	3. dict：切片;
2. 删
	1. str：切片、`lstrip()`、`rstrip()`;
	2. list：`pop()；pop(0)；del arr[id]不存在则报错；remove(value)不存在则报错`;
	3. dict：`del dict[key]删属性;del dict删字典; dict.clear()清条目;`
3. 改
	1. str：`.replace('old','new',max_count)`;
	2. list：切片；
	3. dict：切片;
4. 查找：
	1. str, list, dict：`for i in s`；
	2. str：`find()` 返回id/-1;
	3. list：`index()`返回id/报错，需先判断是否存在;
5. 排序：
	1. str, list, dict ：`sort/sorted`；
6. 计数：
	1. str, list：`count`;
7. 反转：
	1. str：`1)str[::-1]; 2)''.join(reversed(str));`
	2. list：`1)arr[-1::-1]; 2)list(reversed(arr)); 3).reverse();`
8. 遍历：
	1. str：`for i in; `
	2. list：`for i in; for i,v in enumerate(); for i in range; `
	3. dict：`for i in; for i,v in enumerate();`

## 2. 基本数据类型&操作

### 2.1. 字符串

支持的运算：+，*重复，[]索引，[:]切片，左闭右开，in，not in，%格式化；

#### 2.1.1. 基本操作

注意：下属操作，基本不改变原串

1. 拼接/增：
    - +：`c=a+b`
    - join：可迭代对象(字符串、列表、元组、字典name)》字符串：`'sep'.join(iter)`
    - 指定位置插入：转为list，用insert插入后，再转str
2. 删
    - 指定位置：`str[:]=str[:k]+str[k+1:]`
    - 开头+结尾：`s.strip(sep)`,默认为空格;
    - 开头/结尾：`s.lstrip(rm)，s.rstrip(rm)` ;
3. 改    
    - 不改变原串：`newstr = mstr.replace('a','b',max_count)`

5. 查
    - 是否存在：`sub in mstr` #True/False
    - 首次下标：`mstr.find(sub,start,end)` #下标/-1，不能用于lsit;
    - 末次下标：`.rfind(substr)`：#从右开始,找首个，不存在-1;
    - 首次下标：`mstr.index(sub,start,end)`	#下标不存在则抛出异常
    - 末次下标：`mstr.rindex(sub,start,end)`	#下标不存在则抛出异常
    - 以*开始/结束：`str.startwith(),str.endwith` #True，False
    - 切片：`[start:end:step]`，左开右闭，end超出范围，**不报错**；
    - 索引：str[index] **下标不存在则报错**
   
5. 分割：`.split(sep,maxsplit)`，默认None，即所有的空字符（空格、换行'\n'，制表符'\t'等）；
    - sep为空：可匹配连续多个“空格、换行'\n'，制表符'\t'等”；
    - sep不空，每次只匹配1个分隔符；`'hi>>lee'.split('>')`['hi', '', 'lee']；

6. 反转：
    - str[::-1]，
    - ''.join(reversed(str) )

7. 判断
    - 是否大写/小写：`str.islower(),str.isupper(),str.istitle()`#单词首字母大写
    - 是否数字0~9、罗马数字:`isdigit()`;
    - 是否数字0~9、罗马数字、汉字数字：`isnumeric()`,#'十五2三二',True;
    - 是否字母：`isalpha(),isalnum()`#字母、数字或字母
    
8. 转换/运算
    - 大小写转换(创建新数组)：`str.lower(),str.upper()，str.capitalize()，str.title()`
    - 大小写调换（仅字母）：`swapcase()`;
    - 字符串运算：`eval(表达式)`，eval('3*4')  #12
    - 计数：`.count(sub,start,end)`

9. 格式化：
    1. `%s,%d`：%s格式化字符串，%d格式化整数; `print("我叫%s，今年%d岁"%('小明'，20))`
    2. `format`(py2.6开始，**推荐**)：`"{占位符，对应变量下标}".format(var1,...)`，下表从0开始，可省略，变量的数据类型不限(列表、字符串、数值...)
        - `"{0}{1}".format(var1,var2)`或`"{}{}".format(var1,var2)`；
        - `"{0},{1[1]},{2}".format(3,[1,2,3],'bob')` #3,2,bob
    3. 格式化浮点数："{:.2f}".format(要格式化的数字)
        - `{:.2%}.format(0.25)`	#25.00%	百分比格式
        - `{:.0f}.format(2.71828）`	#3	不带小数
        - `{:.2f}.format(3.14159)`	#3.14	保留小数点后两位


```python
mstr = 'hello'
# print(mstr[20]) # 报错

mstr = 'hello'
nstr = mstr.strip('h')
print(nstr,mstr) #ello hello

mstr = 'hello'
c = mstr.replace('l','z')
print(mstr,c) #hello hezzo

mstr = 'hello'
b=''.join(reversed(mstr))
print(b) #olleh

mstr = 'hello'
print(mstr.startswith('z'))
print(mstr.find('he'))

mstr = '十五2三二'
print(mstr.isnumeric())

'cwww.xyz.com'.lstrip('.cwom')
```

    ello hello
    hello hezzo
    olleh
    False
    0
    True
    




    'xyz.com'



### 2.2. 列表[]

1. 定义：**有序**对象集合，元素类型可**不同**；
2. 支持的操作：+拼接，[]索引，[:]切片（左闭右开），in，not in;

#### 2.2.1. 基本操作

1. 创建（3行5列的2维数组）：`arr = [[0]*5 for _ in range (3)]`
2. 增
	- 拼接两列表：`+，extend`
	- 插入，任意位置：`arr.insert(index,value)`
	- 插入，尾：`arr.append(value)`
3. 删
	- 按下标：`del arr[index]` #不存在则报错
   - 按下标：`arr.pop(index)` #不存在则报错
	- 尾部、头部，&返回：`arr.pop(),arr.pop(0)` #不存在则报错
	- 按值，第1个：`arr.remove(value)` #不存在则报错
5. 查
	- 是否存在：`x in arr`: #返回True/False；
	- 首次出现的下标：`arr.index(x,start,end)`，后面2个参数可省；#不存在则报错
6. 遍历
	- 遍历1：`for i in range(n)`,做闭右开，默认从0开始；
	- 遍历2：`for i,v in enumerate(arr,startIndex)`
	- 遍历3：`for i in arr`
7. 反转/转换
	- 返回新对象：`arr[-1: :-1]`
	- 返回新对象：`list(reversed(arr))`
	- 修改原对象：`arr.reverse()》print(arr)`
   - 列表》字符串：newarr = ' '.join(arr)
8. 排序
    1. 修改原对象：`arr.sort(key=lambda x:(x[1],x[0]),reverse=True)`
    2. 返回新对象：`sorted(arr)`
    
9. 运算
    - 计数：`list1.count(item)`
    - 最大/小：`max(list)`
    - 比较：` import operator>> operator.eq(a, b)`
10. 切片：
    - 切片：`[起:终:步长]`，第3个参数可省；若为负，则代表反向截取；
    - 举例：`arr[-1:0:-1]`逆序遍历arr
    - 注意：list的切片操作类似于浅拷贝，对不可变对象的子元素修改或增删不会影响另一对象，对可变对象的子元素操作会影响另一对象。
    - 举例：`a= ['hello',[1,2],'baby']>>b=a[1]>>b.append(3)>>c=a[0]>>c+='U'`：b会影响a，c不会。

### 2.3. 字典{}
1. 定义：无序**键：值**集合，键唯一

#### 2.3.1. 常用操作

1. 获取keys（列表）：dict.keys() 
2. 获取values（列表）：dict.values()
3. 获取items(列表)：  # dic={'a':26,'g':20}; [('a', 26), ('g', 20)]
4. key存在性：key in dict，dict.has_key(key)
5. 添加：dict[key]=value
6. 删除：del dict[key]
7. 删除（所有条目）：dict.clear()，清空所有条目
8. 删除（字典）：del mdict删除字典
9. 修改：dict[key]=newvalue
10. 遍历：for k,val in enumerate(dict)
11. 创建（指定键、值）：`dict.fromkeys(seq,value)`,value可省；
	```python
	seq = ('name', 'age', 'sex')
    print(dict.fromkeys(seq,10))#{'name': 10, 'age': 10, 'sex': 10}
	```
12. 设置默认值： `.setdefault(key, default=None)` 内置方法，设置键值
13. 排序:dict默认无序
    1. itemgetter+sorted：`from operator import itemgetter; sorted(d.items(),key=itemgetter(1),reverse=True)`
    2. lambda+sorted：`sorted(d.items(),key=lambda x:x[1],reverse=True)`;

14. 创建
    - {}，a['key'] = value
    - dict(key1=1,key2='sdf',key3=2.3); 或 dict([ ('key1'=1),('key2'='sdf'),('key3'=2.3) ] ); 
    - {x : x*2 for x in [1,2,3]}  #输出{'1':2,'2':4,'3':6}

#### 2.3.2. collections

1. defaultdict:
    - 功能：设置默认值；
    - 引入：`from collections import defaultdict`
    - 创建：`voc = defaultdict(int/list/set)`，默认值类型
    - 修改：`+=val;.append(val);.add()`
    - 删除：`del voc[key]`

2. orderdict：
    - 功能：有序字典，按创建有序;
    - 创建：fromkeys,`dic = OrderedDict();name=['tom','lucy','sam'];print(dic.fromkeys(name,20))`
    - 按key排序：`kd=OrderedDict(sorted(dd.items(), key=lambda t: t[0]))`
    - 按value排序：`vd=OrderedDict(sorted(dd.items(), key=lambda t: t[1]))`


```python
from collections import defaultdict
voc = defaultdict(int)
voc['name']='lily'
print(voc)
del voc['name']
print(voc)

aa = dict({'a':1,'c':2,'b':3})
print(aa)

from collections import OrderedDict
dd = {'banana': 3, 'apple':4, 'pear': 1, 'orange': 2}
dd['test']=6
dd['apple']=5
# for i in dd.items():
#     print(i)

kd = sorted(dd.items(), key=lambda t: t[0])#按key排序
vd = OrderedDict(sorted(dd.items(),key=lambda t:t[1]))
print(kd)
print(vd)
```

    defaultdict(<class 'int'>, {'name': 'lily'})
    defaultdict(<class 'int'>, {})
    {'a': 1, 'c': 2, 'b': 3}
    [('apple', 5), ('banana', 3), ('orange', 2), ('pear', 1), ('test', 6)]
    OrderedDict([('pear', 1), ('orange', 2), ('banana', 3), ('apple', 5), ('test', 6)])
    

### 2.4. 集合set,{}
1. 定义：元素**不允许重复**
2. 创建：
    - set(list)，set(dict)key元素的集合, set(tuple),
    - 注意：空集合的创建：set()
3. 判断是否包含某元素： i in setA
4. 集合运算：
   1. a-b：(差集)，a.difference(b)集合a中包含,而b中不包含的元素
   2. a|b：(并集)，a.union(b)集合a或集合b包含的所有元素；
   3. a&b：(交集)，a.intersection(b) 集合a和集合b都包含的元素 ；
   4. a^b：(不同时存在的元素)，a.symmetric_difference(b)不同时包含a和b的元素；
   5. 删除a中b的部分：a.difference_update(b)
   6. 判断是否是超集、子集、相交关系;issuperset,issubset,isdisjoint;
5. 增：
    - 加1个：sets.add(val)
    - 批量加：sets.update(list或set)
6. 删：
    - sets.remove()不报错
    - sets.discard()不存在则报错；
7. 拷贝：set2 = set1.copy()



```python
aa = dict({'a':1,'c':2,'b':3})
bb = set(aa)
print(bb)

set1 = set([1,2,3])
set2 = set([3,5,6])
set1.update([4,3])
print(set1)
set1.update(set2)
print(set1)
```

    {'c', 'b', 'a'}
    {1, 2, 3, 4}
    {1, 2, 3, 4, 5, 6}
    

### 2.5. 元组()
1. 定义**无序**，元素不能修改，元素类型可**不同**
2. 创建：atp = (),tup1 = (50,)只包含一个元素时，需要加“,”
3. 访问：下标/切片，左闭右开
3. 拼接：tp3 = tp2+tp1，不允许修改，但可以拼接为新的元组
4. 删除：del tp
5. 查找：x in mtp
6. 遍历：for x in mtp: print x
7. 复制：mtp*4,(3)=>(3,3,3,3)
8. 长度：len(mtp)
9. 列表转元组：tuple(mlist)
10. 最大/小：max()，min()
11. 比较：cmp(tp1,tp2),tp1大，返回1；相等返回0，后者大返回-1；【规则：如果是数字,执行必要的数字强制类型转换,然后比较。如果有一方的元素是数字,则另一方的元素"大"(数字是"最小的")否则,通过类型名字的字母顺序进行比较。】

### 2.6. 堆

- `heapq.heappush(heap,obj)`
- `heapq.heappop(heap)`
- `heapq.heapify(heap)`
- `heapq.nlargest(n,heap)`
- `heapq.nsmallest(n,heap)`

## 3. 解题思路

1. 审题
2. 思路：对应“问题模型”
   1. 字符串
   2. 二叉树
   3. 流程实现
   4. 流程实现
   5. 双指针
   6. 并查集
   7. HASH
   8. DFS/BFS
   9. 单调栈
   10. 滑动窗口
   11. 字典树
3. 代码
   1. 熟悉常用语法、函数
   2. 掌握常用数据结构：
      1. 列表（栈、队列）
      2. 字符串
      3. 字典

## 4. 控制台处理

1. 读取1行：`input()`
2. 读取不定行：
   ```python、
     while True:
        try:
            n = int(input().strip())
            n_bin = bin(n)[2:]
            print(sum([int(i) for i in n_bin]))
        except EOFError:
            break
        except ValueError:
            break
   ```
