[toc]

## Datastructer

### 解压可迭代对象
- 1.1 赋值给变量
    - 规则1：任何可迭代对象，都支持；
    - 规则2：想舍弃部分参数，可用“_”进行接收；
    - 注意：参数数量不匹配(超了)，会报错；

- 1.2 将不定数量的对象，赋值给多个变量
    1. 方法：
        1. **用*pname**接收不定数量的对象，可用在开头、中间、结尾，数据类型为一个**列表**；
        2. 若想用*接收后丢弃，可以用“_”当变量名；
    2. 使用场景：字符串分隔截取等

#### 赋值给变量
- 规则1：任何可迭代对象，都支持；
- 规则2：想舍弃部分参数，可用“_”进行接收；
- 注意：参数数量不匹配(超了)，会报错；


```python
a,b,c=['name',123,(1,2,3)]
print('a,b,c:',a,b,c)
```

    a,b,c: name 123 (1, 2, 3)
    

#### 将不定数量的对象，赋值给多个变量
1. 方法：
    1. **用*pname**接收不定数量的对象，可用在开头、中间、结尾，数据类型为一个**列表**；
    2. 若想用*接收后丢弃，可以用“_”当变量名；
2. 使用场景：字符串分隔截取等


```python
grades = [1,2,3,4,5,6,7]
first, *middle, last = grades
print('first,middle,last:',first,middle,last)
```

    first,middle,last: 1 [2, 3, 4, 5, 6] 7
    

### collections

- 2.1 deque保留最近插入的N个元素
    1. 库：`collections.deque`
    2. 方法：`m_que = deque(arr,maxlen=x)`

- 2.2 Counter 出现次数最多的元素
    1. 库：`collections.Counter`，返回类型：字典
    2. 所有元素频率：`m_cnts = Counter(arr)`,顺序未知；
    3. TOP k：most_common,`m_cnts.most_common(k)`，频率相同，顺序未知；
    4. 更新/增加：update,`m_cnts.update([元素])`
    5. 数学运算，两个统计结果，可直接进行+，-（a-b，a中元素减少，a小则直接去掉）

#### deque保留最近插入的N个元素
1. 库：`collections.deque`
2. 方法：`m_que = deque(arr,maxlen=x)`


```python
from collections import deque
q = deque([1,2,3],maxlen=3)
print(q)
q.extend([4,5])
print(q)
q.appendleft(0)
print(q)
```

    deque([1, 2, 3], maxlen=3)
    deque([3, 4, 5], maxlen=3)
    deque([0, 3, 4], maxlen=3)
    

#### Counter 出现次数最多的元素
1. 库：`collections.Counter`，返回类型：字典
2. 所有元素频率：`m_cnts = Counter(arr)`,顺序未知；
3. TOP k：most_common,`m_cnts.most_common(k)`，频率相同，顺序未知；
4. 更新/增加：update,`m_cnts.update([元素])`
5. 数学运算，两个统计结果，可直接进行+，-（a-b，a中元素减少，a小则直接去掉）


```python
#  统计最大频率
from collections import Counter
arr = ['look', 'into', 'my', 'eyes', 'into', 'my', 'eyes','boy']
m_cnts = Counter(arr)
print('all cnts:',m_cnts)
print('most common 2:',m_cnts.most_common(2))

# 增加某个元素次数
m_cnts.update(['my'])
print('update my:',m_cnts)

# 数学运算
words = ['look', 'my', 'eyes', 'look', 'into', 'my', 'eyes']
new_words = ['why','are','you','look','my','my','my']
cnts = Counter(words)
n_cnts = Counter(new_words)
print('============数学运算：+,- ===============')
print('A:',cnts)
print('B:',n_cnts)
print('A+B',cnts+n_cnts)
print('A-B：',cnts-n_cnts)
```

    all cnts: Counter({'into': 2, 'my': 2, 'eyes': 2, 'look': 1, 'boy': 1})
    most common 2: [('into', 2), ('my', 2)]
    update my: Counter({'my': 3, 'into': 2, 'eyes': 2, 'look': 1, 'boy': 1})
    ============数学运算：+,- ===============
    A: Counter({'look': 2, 'my': 2, 'eyes': 2, 'into': 1})
    B: Counter({'my': 3, 'why': 1, 'are': 1, 'you': 1, 'look': 1})
    A+B Counter({'my': 5, 'look': 3, 'eyes': 2, 'into': 1, 'why': 1, 'are': 1, 'you': 1})
    A-B： Counter({'eyes': 2, 'look': 1, 'into': 1})
    

### 3. heapq 堆
#### 3.1 查找最大或最小的N 个元素
1. 方法：nlargest(n,arr,key) 和 nsmallest(n,arr,key) ，key可省略。

#### 3.2 入堆、出堆
1. heapq.heappush(mqueue,(preorder,index,item))，元素入堆，并保持小根堆的特性；index默认可省略，控制当元素一致时，输出顺序；
2. heapq.heappop(mqueue),返回堆中”最小的” 的元素


```python
import heapq
print(heapq.nlargest(3,[1,2,3,3,4,4]))
```

    [4, 4, 3]
    


```python
import heapq
h = []
heapq.heappush(h, (5, 'write code')) # 5 优先级
heapq.heappush(h, (7, 'release product'))
heapq.heappush(h, (1, 'write spec'))
heapq.heappush(h, (3, 'create tests'))
print('出堆，heapq.heappop，弹最小值：',heapq.heappop(h))
print('最大的2个数，heapq.nlargest：',heapq.nlargest(2,h))
```

    出堆，heapq.heappop，弹最小值： (1, 'write spec')
    最大的2个数，heapq.nlargest： [(7, 'release product'), (5, 'write code')]
    

### 4. 字典
- key对应多个值 defaultdict
    1. 模块：collections.defaultdict
    2. 使用：mdict = defaultdict(int/list/set/dict)

- 字典排序 OrderedDict
    1. 模块：collections.OrderedDict
    2. 使用：mdict = OrderedDict(int/list/set/dict)
    3. 规则：按初次存入的顺序有序

- 字典操作：最大、最小、排序
    1. 取字典中值的最大值`min(zip(prices.values(),prices.keys()))`
    2. 取字典中值的最小值`max(zip(prices.values(),prices.keys()))`
    3. 对字典中值进行排序`sorted(zip(prices.values(),prices.keys()))`
    4. **zip() 函数创建的是一个只能访问一次的迭代器**

- 查找两个字典的相同点
    1. 字典(`mdict.keys(),mdict.items()`等)同样支持集合操作：&，-
    2. 重新构造，排除某些键：字典推导式

#### 设置defaultdict
1. 模块：collections.defaultdict
2. 使用：mdict = defaultdict(int/list/set/dict)


```python
from collections import defaultdict
mdict = defaultdict(list)
mdict['name'].append('lily')
mdict['name'].append('tom')
print(mdict)
```

    defaultdict(<class 'list'>, {'name': ['lily', 'tom']})
    

#### 有序字典OrderedDict
1. 模块：collections.OrderedDict
2. 使用：mdict = OrderedDict(int/list/set/dict)
3. 规则：按初次新建的顺序有序，若中途删除过，则按最后新增的顺序。


```python
from collections import OrderedDict
d = OrderedDict()
d['foo'] = 2
d['bar'] = 4
# del d['foo']
d['foo'] = 2
d['spam'] = 1
d['grok'] = 3
print(d)
```

    OrderedDict([('foo', 2), ('bar', 4), ('spam', 1), ('grok', 3)])
    

#### zip合并操作
1. 取字典中值的最大值`max(zip(prices.values(),prices.keys()))`
2. 取字典中值的最小值`min(zip(prices.values(),prices.keys()))`
3. 对字典中值进行排序`sorted(zip(prices.values(),prices.keys()))`
4. **zip() 函数创建的是一个只能访问一次的迭代器**


```python
prices = {
    'ACME': 45.23,
    'AAPL': 612.78,
    'IBM': 205.55,
    'HPQ': 37.20,
    'FB': 10.75
}
min_price = min(zip(prices.values(),prices.keys()))
max_price = max(zip(prices.values(),prices.keys()))
sort_price = sorted(zip(prices.values(),prices.keys()))
print('最小：',min_price) # (10.75, 'FB')
print('最大：',max_price)# (612.78, 'AAPL')
print('排序后：',sort_price) #

# zip()创建的迭代对象，只能访问一次
temp = zip(prices.values(),prices.keys())
print(min(temp)) # OK
# print(max(temp))# ValueError 
```

    最小： (10.75, 'FB')
    最大： (612.78, 'AAPL')
    排序后： [(10.75, 'FB'), (37.2, 'HPQ'), (45.23, 'ACME'), (205.55, 'IBM'), (612.78, 'AAPL')]
    (10.75, 'FB')
    

#### 字典比较：&和、-差
1. 运算：&，-，字典(`mdict.keys(),mdict.items()`等)同样支持。
2. 重新构造，排除某些键：字典推导式


```python
a = {
    'x' : 1,
    'y' : 2,
    'z' : 3
}
b = {
    'w' : 10,
    'x' : 11,
    'y' : 2
}
# 找相同
print('key相同：',a.keys()&b.keys()) #{'x','y'}
print('items相同：',a.items() & b.items() )# { ('y', 2) })
# 重新构造：推导式
c = {key:a[key] for key in a.keys() - {'z', 'w'}}
print('对a中key进行过滤：',c) # {'y': 2, 'x': 1}
```

    key相同： {'x', 'y'}
    items相同： {('y', 2)}
    对a中key进行过滤： {'x': 1, 'y': 2}
    

#### 利用字典构造子集
1. 方法1：字典推导
    - `{key: value for key, value in mdict.items()}`
2. 方法2：创建元组序列，再传给dict()
    - `dict((key, value) for key, value in prices.items() if value > 200)`
3. 区别：方法1更快。


```python
prices = {
'ACME': 45.23,
'AAPL': 612.78,
'IBM': 205.55,
'HPQ': 37.20,
'FB': 10.75
}
print('做值的的过滤')
p1 = {key: value for key, value in prices.items() if value > 200}
print(p1)
print()
print('做key的过滤')
tech_names = {'AAPL', 'IBM', 'HPQ', 'MSFT'}
p2 = {key: value for key, value in prices.items() if key in tech_names}
print(p2)
```

    做值的的过滤
    {'AAPL': 612.78, 'IBM': 205.55}
    
    zipkey的过滤
    {'AAPL': 612.78, 'IBM': 205.55, 'HPQ': 37.2}
    

#### 合并多个字典/映射
1. 方法1：`collections.ChainMap(dict1,dict2...)`。
    1. 原理：只是逻辑上将多个字典合并为一个，实际是创建了一个列表，并重新定义了列表的操作（如：`len(c),c.keys(),c.values()`）。
    2. 查找：针对所有字典，但优先返回“靠前的字典”中的值。
    3. 更新、删除：只针对列表中第一个字典。(删除时若不存在，会报错）
2. 方法2：`dict1.update(dict2)`
    1. 弊端：若dict2改变，dict1不会更新


```python
from collections import ChainMap
a = {'x':1,'z':3}
b = {'y':2,'z':4}
print('========ChainMap方法===============')
c = ChainMap(a,b)
print('初始的c',c)
print('c的长度len:',len(c))
print('c的keys:',list(c.keys()))
print('c的values:',list(c.values()))
print()
print('查找c[z]',c['z'])
print('查找c[y]',c['y'])
print('删、改只影响第一个字典，不存在则报错')
print()
c['y']=666
print('修改y后的c',c)
del c['y'] #若提到前面，会报错
print('c',c)
print()
print('========update方法===============')
new_dict = dict(b)
new_dict.update(a)
print('a,b',a,b)
print('new_dict',new_dict)
a['z']=555
print('a',a)
print('new_dict',new_dict)
```

    ========ChainMap方法===============
    初始的c ChainMap({'x': 1, 'z': 3}, {'y': 2, 'z': 4})
    c的长度len: 3
    c的keys: ['y', 'z', 'x']
    c的values: [2, 3, 1]
    
    查找c[z] 3
    查找c[y] 2
    删、改只影响第一个字典，不存在则报错
    
    修改y后的c ChainMap({'x': 1, 'z': 3, 'y': 666}, {'y': 2, 'z': 4})
    c ChainMap({'x': 1, 'z': 3}, {'y': 2, 'z': 4})
    
    ========update方法===============
    a,b {'x': 1, 'z': 3} {'y': 2, 'z': 4}
    new_dict {'y': 2, 'z': 3, 'x': 1}
    a {'x': 1, 'z': 555}
    new_dict {'y': 2, 'z': 3, 'x': 1}
    

### list
- 5.1 删除序列相同元素并保持顺序
    1. 借助set，遍历；
    2. 注意：这个方法仅仅在序列中**元素为hashable**的时候才管用；
    3. 注：hashable：可散列，即不可变，如str，数值；
    4. 对字典进行排序，若键不完全相同，会报错。

- 5.2 通过某个关键字排序一个字典列表
    1. 模块：`operator.itemgetter`
    2. 方法：`res = sorted(arr_dict,key=itemgetter(key1,key2...))`

#### 去重（保持原始顺序）
1. 借助set，遍历；
2. 注意：这个方法仅仅在序列中**元素为hashable**的时候才管用；
3. 注：hashable：可散列，即不可变，如str，数值；


```python
# 5.1 元素为hashable，如数值型
def remove_duplicate(items, key=None):
    seen = set()
    for item in items:
        val = item if key is None else key(item)
        if val not in seen:
            yield item
            seen.add(val)
a = [1, 5, 2, 1, 9, 1, 5, 10]
print('原始list：',a)
print('去重后list:',list(remove_duplicate(a))) # [1, 5, 2, 9, 10])

# 元素非hashable，如字典
def remove_duplicate1(items, key=None):
    seen = set()
    for item in items:
        val = item if key is None else key(item)
        if val not in seen:
            yield item
            seen.add(val)
    print('seen',seen)
a = [ {'x':1, 'y':2}, {'x':1, 'y':3}, {'x':1, 'y':2}, {'x':2, 'y':4}]
print('原始dict：',a)
print('去重后dict:',list(remove_duplicate1(a, key=lambda d: (d['x'],d['y']))))
```

    原始list： [1, 5, 2, 1, 9, 1, 5, 10]
    去重后list: [1, 5, 2, 9, 10]
    原始dict： [{'x': 1, 'y': 2}, {'x': 1, 'y': 3}, {'x': 1, 'y': 2}, {'x': 2, 'y': 4}]
    seen {(1, 2), (1, 3), (2, 4)}
    去重后dict: [{'x': 1, 'y': 2}, {'x': 1, 'y': 3}, {'x': 2, 'y': 4}]
    

#### 字典列表排序
1. 方法1：`operator.itemgetter`,`res = sorted(arr_dict,key=itemgetter(key1,key2...))`
2. 方法2：lambda表达式.
3. 区别：itemgetter方法，比lambda表达式更快。
4. 拓展：itemgetter可与min,max结合使用


```python
from operator import itemgetter
rows = [
    {'fname': 'Brian', 'lname': 'Jones', 'uid': 1003},
    {'fname': 'David', 'lname': 'Beazley', 'uid': 1002},
    {'fname': 'Big', 'lname': 'Jones', 'uid': 1004}
]
rows_by_fname = sorted(rows, key=itemgetter('lname','fname'))
print(rows_by_fname)
```

    [{'fname': 'David', 'lname': 'Beazley', 'uid': 1002}, {'fname': 'Big', 'lname': 'Jones', 'uid': 1004}, {'fname': 'Brian', 'lname': 'Jones', 'uid': 1003}]
    

#### 分组
1. 方法：`itertools.groupby(序列，key=itemgetter('列名'))`
2. 注意：通常需先进行排序，与operator的itemgetter结合使用


```python
from operator import itemgetter
from itertools import groupby
rows = [
{'address': '5412 N CLARK', 'date': '07/01/2012'},
{'address': '5148 N CLARK', 'date': '07/04/2012'},
{'address': '5800 E 58TH', 'date': '07/02/2012'},
{'address': '2122 N CLARK', 'date': '07/03/2012'},
{'address': '5645 N RAVENSWOOD', 'date': '07/02/2012'},
{'address': '1060 W ADDISON', 'date': '07/02/2012'},
{'address': '4801 N BROADWAY', 'date': '07/01/2012'},
{'address': '1039 W GRANVILLE', 'date': '07/04/2012'},
]
rows.sort(key=itemgetter('date'))
for date,items in groupby(rows,key=itemgetter('date')):
    print(date)
    for i in items:
        print(' ',i)
```

    07/01/2012
      {'address': '5412 N CLARK', 'date': '07/01/2012'}
      {'address': '4801 N BROADWAY', 'date': '07/01/2012'}
    07/02/2012
      {'address': '5800 E 58TH', 'date': '07/02/2012'}
      {'address': '5645 N RAVENSWOOD', 'date': '07/02/2012'}
      {'address': '1060 W ADDISON', 'date': '07/02/2012'}
    07/03/2012
      {'address': '2122 N CLARK', 'date': '07/03/2012'}
    07/04/2012
      {'address': '5148 N CLARK', 'date': '07/04/2012'}
      {'address': '1039 W GRANVILLE', 'date': '07/04/2012'}
    

#### 过滤（自定义规则）
1. 方法1：列表推导式，`[x for x in oldlist if n>0)`
2. 方法2：“生成器表达式”，迭代产生要过滤的元素，`items = (x for x in oldllist if x>0)`
3. 方法3：利用内建的filter()+过滤规则提取到函数中。
4. 对比：
    - 方法1：数据量大时，占内存。
    - 方法2：内存小，但不适用于复杂的过滤规则。
    - 方法3：适用于复杂的过滤逻辑。


```python
mylist = [1, 4, -5, 10, -7, 2, 3, -1]
print('***********方法1***********')
new1=[n for n in mylist if n > 0]
print(new1)
print('***********方法2***********')
new2 = (n for n in mylist if n > 0)
for i in new2:
    print(i)
print('***********方法3***********')
mylist1 = ['1', '2', '-3', '-', '4', 'N/A', '5']

# 过滤非数字，保留数字
def fun(val):
    try:
        x = int(val)
        return True
    except ValueError:
        return False
print('old value:',mylist1)
new3 = list(filter(fun, mylist1))
print('new value:',new3)
```

    ***********方法1***********
    [1, 4, 10, 2, 3]
    ***********方法2***********
    1
    4
    10
    2
    3
    ***********方法3***********
    old value: ['1', '2', '-3', '-', '4', 'N/A', '5']
    new value: ['1', '2', '-3', '4', '5']
    

#### 过滤（用另一个关联序列过滤）
1. 方法：`itertools.compress(可迭代对象，布尔序列)`


```python
from itertools import compress
cnt = [0,3,2,1]
address = ['2312 bei','3333 ci','2222 lai','666 liang']
rules = [x>1 for x in cnt]
new_address = list(compress(address,rules))
print(new_address)
```

    ['3333 ci', '2222 lai']
    

#### 使用聚合函数（转换并计算数据）
1. 目的：对一个数组，求每个元素的平方和
2. 方法1：借助生成器表达式，占内存少（返回的为迭代器） `sum(x*x for x in arr)`
3. 方法2：借助推导式，但占内存多，不推荐。`sum([x*x for x in nums])`

### 切片
1. 切片命名`slice_name = slice(start,end,step)`,
2. 取值：`arr[slice_name]`
3. 修改切片位置的元素（原数组会改变）：`arr[slice_name]=[...]`
4. 删除切片位置的元素（原数组会改变）：`del arr[slice_name]`
5. 切片规格：`slice_name.start/stop/step`
6. 按数组长度，切片自适应长度调整：`new_slice = slice_name.indices(mlen)`


```python
SCORE=slice(4,10)
NAME = slice(0,4)
record = 'lily1314'
print('用切片，取值：',record[NAME]+":"+record[SCORE])
print()
n_slice = slice(3,100,2)
print('调整前slice规模：',n_slice)
a = n_slice.indices(5)
print('调整后slice规模：',a)
```

    用切片，取值： lily:1314
    
    调整前slice规模： slice(3, 100, 2)
    调整后slice规模： (3, 5, 2)
    

### 对象

#### 对象排序
1. 方法1：lambda表达式
2. 方法2：attrgetter，`from operator import attrgetter`,`sorted(mobject, key=attrgetter('key1','key2'))`
3. 区别：itemgetter方法，比lambda表达式更快。
4. 拓展：itemgetter可与min,max结合使用


```python
class User:
    def __init__(self, user_id):
        self.user_id = user_id
    def __repr__(self):
        return 'User({})'.format(self.user_id)
def sort_notcompare():
    users = [User(23), User(3), User(99)]
    users1 = [User(23), User(3), User(99)]
    print('原始user:',users)
    print('lambda表达式排序:',sorted(users, key=lambda u: u.user_id))
    from operator import attrgetter
    print('attrgetter排序:',sorted(users1, key=attrgetter('user_id')))
sort_notcompare()
```

    原始user: [User(23), User(3), User(99)]
    lambda表达式排序: [User(3), User(23), User(99)]
    attrgetter排序: [User(3), User(23), User(99)]
    
