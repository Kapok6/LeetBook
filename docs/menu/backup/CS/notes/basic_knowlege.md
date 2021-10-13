```python
class Test():
    def fun(self,data=[]):
        data.sort()
        data.append('end')
        return data
if __name__ == "__main__":
    t1 = Test()
    print(t1.fun())
    t2 = Test()
    print(t2.fun())
    t3 = Test()
    print(t3.fun(data=['name:123','age:12']))
    t4 = Test()
    print(t4.fun())
```

    ['end']
    ['end', 'end']
    ['age:12', 'name:123', 'end']
    ['end', 'end', 'end']
    

global和nonlocal
1. global：在内层修改全局变量时使用
2. nonlocal：在内层修改外面一层函数中定义的变量(非全局变量）
3. 变量引用顺序：
   1. 当前作用域局部变量
   2. 外层作用域变量nonlocal
   3. 当前模块全局变量
   4. 内置变量


```python
n = 0
def f():
    def inner():
        nonlocal n # 外层局变
        n = 2
    n = 1
    print(n) # 1
    inner()
    print(n) #2
if __name__ == "__main__":
    f()
    print(n)
```

    1
    2
    0
    


```python
def nonlocal_test():
    count = 0
    print('cnt',count)
    def test2():
        nonlocal count
        print('test',count)
        count += 1
        return count
    return test2
 
val = nonlocal_test()
print(val()) # 只调用test2
print(val())
print(val())
```

    cnt 0
    test 0
    1
    test 1
    2
    test 2
    3
    


```python
def scope_test():
    def do_local():
        spam = 'local'# 修改无效
    def do_nonlocal():
        nonlocal spam
        spam = 'nonlocal'
    def do_global():
        global spam
        spam = 'global'
    spam = 'test spam'
    do_local() # 修改无效
    print('after local:',spam)
    do_nonlocal()
    print('after nonlocal:', spam)
    do_global()# 修改有效，但在函数内部调动，优先取局变》外层》全局》内置
    print('after global:',spam)
    
scope_test()
print('in global scope:',spam)
```

    after local: test spam
    after nonlocal: nonlocal
    after global: nonlocal
    in global scope: global
    

### 在函数内改变传参的值，是否改变原参数的值？
**结论：不同参数类型，不同**
1. 数值、字符串：不改变
2. 列表、字典、集合：改变


```python
def fun(num,mstr,arr,mdict,mset):
    num = 2
    mstr += ' lily'
    arr.append(1)
    mdict['name'] = 'lily'
    mset.add(6)
num = 1
mstr = 'hello'
arr = [2,3]
mdict = {'age':3}
mset = set([1,2,3])
fun(num,mstr,arr,mdict,mset)
print(num,mstr,arr,mdict,mset)
```

    1 hello [2, 3, 1] {'age': 3, 'name': 'lily'} {1, 2, 3, 6}
    

### 索引
1. 左闭右开 arr[-5:-1]，不包含倒数第1个


```python
mstr = 'hello world'
print(mstr[-5:])
```

    world
    

迭代器
1. next()


```python
arr = [1,2,3]
y = (x*x for x in arr)
print(next(y),next(y))
```

    1 4
    

### 局变作用域
1. 


```python
i = 3
def pro(y):
    # do somethinig
    pass
def fun(x):
    def bar():
        return i
    for i in x:
        pro(x)
    return bar()
i = 3
def fun1(x):
    def bar():
        return i
    y = [i for i in x]
    pro(y)
    return bar()
print("%s,%s"%(fun(["x","y"]),fun1(["x","y"])))
```

    y,3
    

### __all__
1. python模块中的__all__属性，可用于模块**模糊导入**时限制，如：
2. 模糊导入：from module import * 
3. 精确导入：from module import xxx,yyy
    - 此时被导入模块若定义了__all__属性，则只有__all__内指定的属性、方法、类可被导入。
    - 若没定义，则导入模块内的所有公有属性，方法和类 。


```python
# test.py
__all__ = ['A']
class A(object):
    def __init(self):
        self.name = 'a'
class B(object):
    def __init(self):
        self.name = 'b' 

# case 1
from test import A,B
def test1():
    a = A()
    b= B()
    print(a.name,b.name) # a,b

# case 2
from test1 import *
def test2():
    a = A()
    b= B()
    print(a.name)   
    print(a.name,b.name) # 报错
test2()       
```


```python

```
