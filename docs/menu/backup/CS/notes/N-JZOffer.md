1. 【数组】二维数组中的查找
    在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。
	
	```python
    def Find(self, target, array):
        rows = len(array)
        cols = len(array[0])
        row = 0
        col = cols-1
        icon = False
        while (row<rows)&(col>=0):
            if (array[row][col]==target):
                icon = True
                break
            elif array[row][col]>target:
                col = col-1
            else:
                row = row+1
        return icon
	```
2. 【字符串】替换空格
	请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。
	```
	def replaceSpace(self, s):
        r=""
        s=s.split(" ")
        n=len(s)
        for i in s[:n-1]:
            r=r+i+"%20"
        r+=s[n-1]
        return r
	```
3. 【链表】从尾到头打印链表
	输入一个链表，按链表从尾到头的顺序返回一个ArrayList。
	```
	def printListFromTailToHead(self, listNode):
        mlist = []
        while listNode:
            mlist.insert(0,listNode.val)
            listNode = listNode.next
        return mlist
	```
4. 【树】重建二叉树
	输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。
	```
	def reConstructBinaryTree(self, pre, tin):
        if len(pre) == 0:
            return None
        elif len(pre) == 1:
            return TreeNode(pre[0])
        else:
            troot = TreeNode(pre[0])
            troot.left = self.reConstructBinaryTree(
                 pre[1:tin.index(pre[0])+1],tin[:tin.index(pre[0])])
            troot.right = self.reConstructBinaryTree(
                 pre[tin.index(pre[0])+1:],tin[tin.index(pre[0])+1:])
        return troot
	```
5. 【栈和队列】用两个栈实现队列
	用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。
	```
	class Solution:
        def __init__(self):
            self.s1 = []
            self.s2 = []
        def push(self, node):
            # write code here
            self.s1.append(node)
        def pop(self):
            # return xx
            if self.s2 == []:
                while self.s1:
                    self.s2.append(self.s1.pop())
                return self.s2.pop()
            return self.s2.pop()
	```
6. 【查找和排序】旋转数组的最小数字
	把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个非递减排序的数组的一个旋转，输出旋转数组的最小元素。例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。
	```
class Solution:
    def minNumberInRotateArray(self, rotateArray):
        if rotateArray==None:
            return 0
        mlen = len(rotateArray)
        mleft = 0
        mright = mlen-1
        mid = 0
        # 二分查找变动的是指针
        while rotateArray[mleft]>=rotateArray[mright]:#注意等号
            if mright == (mleft+1):#终止条件
                mid = mright #存储下标
                break
            mid = (mleft+mright)/2
            # bug：左、右、中间元素，三者相等，无法继续二分，则遍历找最小
            if (rotateArray[mleft]==rotateArray[mright]) and
                               (rotateArray[mid]==rotateArray[mright]):
                return self.searchMin(rotateArray)
            if rotateArray[mid]>=rotateArray[mleft]:#注意等号
                mleft = mid
            else:
                mright = mid
        return rotateArray[mid]
    def searchMin(self,arr):
        minarr = arr[0]
        for i in arr:
            if i< minarr:
                minarr = i
        return minarr
	```
7. 【递归和循环】斐波那契数列
	大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0）。n<=39
	```
	def Fibonacci(self, n):
        if n<=0:
            return 0
        elif n<2:
            return n
        else:
            f1 = 0
            f2 = 1
            for i in range(n-1):
                fin = f1+f2
                f1 = f2
                f2 = fin
            return fin
	```
8. 【递归和循环】跳台阶
	一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法（先后次序不同算不同的结果）
	```
	    def jumpFloor(self, number):
            if number<1:
                return -1
            elif (number==1 or number==2):
                return number
            else:
                f = 1
                s = 2
                t = 0
                for i in range(2,number):
                    t = f+s
                    f = s
                    s =t
                return t
	```
9. 【递归和循环】变态跳台阶
	一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。
    ```
	def jumpFloorII(self, number):
        if number<1:
            return 0
        elif number<3:
            return number
        else:
            res = 2
            for i in range(2,number):
                res = 2*res
            return res
    ```
10. 【递归和循环】矩形覆盖
	我们可以用2*1的小矩形横着或者竖着去覆盖更大的矩形。请问用n个2*1的小矩形无重叠地覆盖一个2*n的大矩形，总共有多少种方法？
	```
	def rectCover(self, number):
        if number<1:
            return 0
        elif number<3:
            return number
        else:
            f1 = 1
            f2 = 2
            for i in range(2,number):
                res = f1+f2
                f1 = f2
                f2 = res
            return res
	```
11. 【位运算】二进制中1的个数
	输入一个整数，输出该数二进制表示中1的个数。其中负数用补码表示。
	```
	def NumberOf1(self, n):
        count = 0
        if n<0:
            n=n&0xFFFFFFFF
        while n:
            n = n&(n-1)
            count+= 1
        return count
	```
12.【代码完整性】数值的整数次方
	给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。保证base和exponent不同时为0。
	```
	def Power(self, base, exponent):
        if exponent == 0:
            return 1
        else:
            mabs = abs(exponent)
            res = 1
            for i in range(mabs):
                res *= base
            if exponent>0:
                return res
            else:
                return 1.0/res
	```
13.【代码完整性】调整数组顺序使奇数位于偶数前面
	输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。
	```
	def reOrderArray(self, array):
        if array==None:
            return None
        modd = 0
        for i in array:
            if i%2!=0:
                modd += 1
        if modd==0 or modd==len(array):
            return array
        else:
            marray = [0]*len(array)
            nodd = 0
            neven = modd
            for i in array:
                if i%2!=0:
                    marray[nodd] = i
                    nodd += 1
                else:
                    marray[neven] = i
                    neven += 1
            return marray
	```
14.【代码鲁棒性】链表中倒数第k个结点
	输入一个链表，输出该链表中倒数第k个结点。
	```
	def FindKthToTail(self, head, k):
        if head== None:
            return head
        if k<=0:
            return ListNode(0).next
        p1=head
        p2=head
        for i in range(k-1):
            if p1.next:
                p1=p1.next
            else:
                return p1.next
        while p1.next:
            p1=p1.next
            p2=p2.next
        return  p2
	```
15.【代码鲁棒性】反转链表
	输入一个链表，反转链表后，输出新链表的表头。
	```
	def ReverseList(self, pHead):
        if (not pHead) or (not pHead.next):
            return pHead
        else:
            last = None
            while pHead:
                tmp = pHead.next
                pHead.next = last
                last = pHead
                pHead = tmp
            return last
	```
16.【代码鲁棒性】合并两个排序的链表
	输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。
	```
	def Merge(self, pHead1, pHead2):
        mergehead = ListNode(1)
        p = mergehead
        while pHead1 and pHead2:
            if pHead1.val<=pHead2.val:
                mergehead.next = pHead1
                pHead1 = pHead1.next
            else:
                mergehead.next = pHead2
                pHead2 = pHead2.next
            mergehead = mergehead.next
        if pHead1:
            mergehead.next = pHead1
            pHead1 = pHead1.next
        elif pHead2:
            mergehead.next = pHead2
            pHead2 = pHead2.next
        return p.next #很关键
	```
17.【代码鲁棒性】树的子结构
	输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）
	```
	class Solution:
        def Subtree(self, pRoot1, pRoot2):
            if not pRoot1 or not pRoot2:
                return False
            return self.check(pRoot1,pRoot2) or self.Subtree(pRoot1.left,pRoot2) or self.Subtree(pRoot1.right,pRoot2)
        def check(self,A,B):
            if not B:
                return True
            if not A or A.val!=B.val:
                return False
            return self.check(A.left,B.left) and self.check(A.right,B.right)
	```
18.【面试思路】二叉树的镜像
	操作给定的二叉树，将其变换为源二叉树的镜像。
	```
	def Mirror(self,root):
        if(root==None):
            return
        alist = []
        alist.append(root)
        while len(alist)!=0:
            cur = alist.pop(0)
            if(cur.left!=None or cur.right!=None):
                cur.left,cur.right = cur.right,cur.left
            if(cur.left):
                alist.append(cur.left)
            if(cur.right):
                alist.append(cur.right)
	```
19.【借助画图】顺时针打印矩阵
	输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字，例如，如果输入如下4 X 4矩阵： 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 则依次打印出数字1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10.
	```
	class Solution:
        # matrix类型为二维列表，需要返回列表
        def printMatrix(self, matrix):
            # write code here
            alist = []
            while matrix:
                alist +=matrix.pop(0) #两个list拼接，用+
                if not matrix or not matrix[0]:
                    break
                matrix = self.turnMatrix(matrix)#逆时针旋转90度
            return alist
        def turnMatrix(self,matrix):
            mrow = len(matrix)
            mcol = len(matrix[0]) #matrix无shape属性
            mlist1 = []
            for i in range(mcol):
                mlist2 = []
                for j in range(mrow):
                    # 从左开始，按列存储
                    mlist2.append(matrix[j][i])
                mlist1.append(mlist2)
            mlist1.reverse()#矩阵水平翻转
            return mlist1
	```
20.【借助举例】包含min函数的栈
	定义栈的数据结构，请在该类型中实现一个能够得到栈中所含最小元素的min函数（时间复杂度应为O（1））。
	```
	class Solution:
        def __init__(self):
            self.stack = []
            self.minstack = []
        def push(self, node):
            # write code here
            self.stack.append(node)
            if len(self.minstack)==0 or node<=self.minstack[-1]:
                self.minstack.append(node)
        def pop(self):
            # write code here
            if self.stack[-1]==self.minstack[-1]:
                self.minstack.pop(-1)
            self.stack.pop(-1)
        def top(self):
            # write code here
            return self.stack[-1]
        def min(self):
            # write code here
            return self.minstack[-1]
	```
21.【借助举例】栈的压入、弹出序列
	输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否可能为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如序列1,2,3,4,5是某栈的压入顺序，序列4,5,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。（注意：这两个序列的长度是相等的）
	```
	def IsPopOrder(self, pushV, popV):
        if not pushV or len(pushV)!= len(popV):
            return False
        stack = []
        for i in pushV:
            stack.append(i)
            while len(stack) and stack[-1] == popV[0]:
                stack.pop() # 弹出最后一个
                popV.pop(0) # 弹出第一个
        if len(stack):
            return False
        return True
	```
22.【借助举例】从上往下打印二叉树
	从上往下打印出二叉树的每个节点，同层节点从左至右打印。
	```
	def PrintFromTopToBottom(self, root):
        if not root:
            return []
        l = []
        q = [root]
        while q:
            temp = q.pop(0)
            l.append(temp.val)
            if temp.left:
                q.append(temp.left)
            if temp.right:
                q.append(temp.right)
        return l
	```
23.【借助举例】二叉搜索树的后序遍历序列
	输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。如果是则输出Yes,否则输出No。假设输入的数组的任意两个数字都互不相同。
	```
	def VerifySquenceOfBST(self, sequence):
        if not sequence:
            return False
        seqLen = len(sequence)
        if seqLen == 1:
            return True
        tempRoot = sequence[-1]
        k = 0
        while sequence[k]<tempRoot:
            k += 1
        for i in range(k,seqLen-1):
            if sequence[i]<tempRoot:
                return False
        leftTree = sequence[:k]
        rightTree = sequence[k:seqLen-1]
        leftIcon = True
        rightIcon = True
        if len(leftTree):
            leftIcon = self.VerifySquenceOfBST(leftTree)
        if len(rightTree):
            rightIcon = self.VerifySquenceOfBST(rightTree)
        return leftIcon and rightIcon
	```
24.【借助举例】二叉树中和为某一值的路径
	输入一颗二叉树的跟节点和一个整数，打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。(注意: 在返回值的list中，数组长度大的数组靠前)
	```
	def FindPath(self, root, expectNumber):
        if not root:
            return []
        if not root.left and not root.right and root.val==expectNumber:
            return [[root.val]]
        pathlist = []
        temp = []
        left = self.FindPath(root.left,expectNumber-root.val)
        right = self.FindPath(root.right,expectNumber-root.val)
        for i in left+right:
            pathlist.append([root.val]+i)
        return pathlist
	```
25.【分解问题】复杂链表的复制
	输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，另一个特殊指针指向任意一个节点），返回结果为复制后复杂链表的head。（注意，输出结果中请不要返回参数中的节点引用，否则判题程序会直接返回空）
	```
	def Clone(self, pHead):
        if not pHead:
            return pHead
        newnode = RandomListNode(pHead.label)
        newnode.next = pHead.next
        newnode.random = pHead.random
        newnode.next = self.Clone(pHead.next)
        return newnode
	```
26.【分解问题】二叉搜索树与双向链表
	输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。要求不能创建任何新的结点，只能调整树中结点指针的指向。
	```
	def Convert(self, pRootOfTree):
        if not pRootOfTree:
            return pRootOfTree
        if not pRootOfTree.left and not pRootOfTree.right:
            return pRootOfTree
        left = self.Convert(pRootOfTree.left)
        p = left
        while p and p.right:
            p = p.right
        if left:
            p.right = pRootOfTree
            pRootOfTree.left = p
        right = self.Convert(pRootOfTree.right)
        if right:
            right.left = pRootOfTree
            pRootOfTree.right =right
        if left:
            return left
        else:
            return pRootOfTree
	```
27.【分解问题】字符串的排列
	输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。
	```
	def Permutation(self,ss):
        if not ss:
            return []
        if len(ss)==1:
            return list(ss) # 返回列表形式的字符串
        allList = []
        charList = list(ss)
        charList.sort()  #按字典顺序进行排列，不去重
        for i in range(len(charList)):
            # 限制i>0 : 防止charList[i-1]下标越界
            if i>0 and charList[i]==charList[i-1]:
                continue # 忽略重复的字符
            #Permutation():乱序，接受array类型及int类型的输入。
            #shuffle():乱序，只接受array类型输入。
            temp = self.Permutation(''.join(charList[:i])+''.join(charList[i+1:]))
            for j in temp:
                allList.append(charList[i]+j)
        return allList
	```
28.【时间效率】数组中出现次数超过一半的数字
	数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。例如输入一个长度为9的数组{1,2,3,2,2,2,5,4,2}。由于数字2在数组中出现了5次，超过数组长度的一半，因此输出2。如果不存在则输出0。
	```
	def MoreThanHalfNum_Solution(self, numbers):
        if numbers == None:
            return 0
        cnt = 1
        temp = numbers[0]
        mlen = len(numbers)
        for i in range(1,mlen):
            if numbers[i]==temp:
                cnt += 1
            else:
                cnt -= 1
            if cnt == 0:
                temp = numbers[i]
                cnt = 1 #注意
        cnt = 0
        for i in numbers:
            if i == temp:
                cnt += 1
        if cnt>mlen/2: #注意，不能用cnt>mlen/2
            return temp
        else:
            return 0
	```
29.【时间效率】最小的K个数
	输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4,。
	```
	def GetLeastNumbers_Solution(self, tinput, k):
        #此方法时间复杂度为O(nlogn)
        if k >len(tinput) or not tinput:
            return []
        #tinput.sort()
        #实现一个快速排序
        def quick_sort(array):
            if not array:
                return []
            pivot=array[0]
            left=quick_sort([x for x in array[1:] if x<pivot])
            right=quick_sort([x for x in array[1:] if x>=pivot])
            return left+[pivot]+right
         
        return quick_sort(tinput)[:k]
	def GetLeastNumbers_Solution1(self, tinput, k):
        if not tinput:
            return []
        if len(tinput)<k:
            return []
        # sorted 与 sort区别
        # sort()在本地进行排序，不返回副本
        # sorted() 返回副本，原始输入不变
        tinput = sorted(tinput)
        return tinput[:k]
	```
30.【时间效率】连续子数组的最大和
	HZ偶尔会拿些专业问题来忽悠那些非计算机专业的同学。今天测试组开完会后,他又发话了:在古老的一维模式识别中,常常需要计算连续子向量的最大和,当向量全为正数的时候,问题很好解决。但是,如果向量中包含负数,是否应该包含某个负数,并期望旁边的正数会弥补它呢？例如:{6,-3,-2,7,-15,1,2,2},连续子向量的最大和为8(从第0个开始,到第3个为止)。给一个数组，返回它的最大连续子序列的和，你会不会被他忽悠住？(子向量的长度至少是1)
	```
	def FindGreatestSumOfSubArray(self, array):
        if array == None:
            return None
        res = 0
        greatsum = float('-inf') #负无穷大
        for i in array:
            if res<=0: #判断的为上一次的求和结果
                res = i
            else:
                res += i
            if res>greatsum:
                greatsum = res
        return greatsum
	```
31.【时间效率】整数中1出现的次数（从1到n整数中1出现的次数）
	求出1~13的整数中1出现的次数,并算出100~1300的整数中1出现的次数？为此他特别数了一下1~13中包含1的数字有1、10、11、12、13因此共出现6次,但是对于后面问题他就没辙了。ACMer希望你们帮帮他,并把问题更加普遍化,可以很快的求出任意非负整数区间中1出现的次数（从1 到 n 中1出现的次数）。
	```
	def NumberOf1Between1AndN_Solution(self, n):
        if n<=0:
            return 0
        base = 1
        res = 0
        while n/base:
            res += n/(10*base)*base
            last = (n/base)%10 #获取当前位上的值
            if last == 1:
                res += n%base+1
            elif last>1:
                res += base
            base *= 10
        return res

	```
32.【时间效率】把数组排成最小的数
	输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。
	```
	def PrintMinNumber(self, numbers):
        if numbers == None:
            return ''
        numbs = map(str,numbers) #类型批量转换map
        numbs.sort(lambda x,y:cmp(x+y,y+x)) #默认升序，参数cmp=可省略
        return ''.join(numbs) #将list拼接为字符串
	```
33.【时空平衡】丑数
	把只包含质因子2、3和5的数称作丑数（Ugly Number）。例如6、8都是丑数，但14不是，因为它包含质因子7。 习惯上我们把1当做是第一个丑数。求按从小到大的顺序的第N个丑数。
	```
	def GetUglyNumber_Solution(self, index):
        if index<1:
            return 0
        uglyList=[1]
        id2,id3,id5=[0,0,0]
        for i in range(index-1):
            res = min(uglyList[id2]*2,uglyList[id3]*3,uglyList[id5]*5)
            uglyList.append(res)
            if (res%2==0):
                id2+=1
            if (res%3==0):
                id3+=1
            if (res%5==0):
                id5+=1
        return uglyList[-1]
	```
34.【时空平衡】第一个只出现一次的字符位置
	在一个字符串(0<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符,并返回它的位置, 如果没有则返回 -1（需要区分大小写）.
	```
	def FirstNotRepeatingChar(self, s):
        for k,i in enumerate(s):
            if s.count(i)==1:
                return k
        return -1
	```
35.【时空平衡】数组中的逆序对
	
36.【时空平衡】两个链表的第一个公共结点
	输入两个链表，找出它们的第一个公共结点。
	```
	def FindFirstCommonNode(self, pHead1, pHead2):
        p1 = pHead1
        p2 = pHead2
        while(p1!=p2):
            p1 = pHead2 if p1==None else p1.next
            p2 = pHead1 if p2==None else p2.next
        return p1
	```
37.【知识迁移】数字在排序数组中出现的次数
统计一个数字在排序数组中出现的次数。
	```
	def GetNumberOfK(self, data, k):
        if data == []:
            return 0
        mlen = len(data)
        l = 0
        r = mlen-1
        counts = 0
        while l<=r:
            mid = (l+r)/2
            if data[mid]==k:
                counts += 1
                r = mid+1
                l = mid-1
                while r<mlen and data[r]==k:
                    counts += 1
                    r += 1
                while l>=0 and data[l]==k:
                    counts += 1
                    l -= 1
            elif data[mid]>k:
                r = mid-1
            else:
                l = mid+1
        return counts
	```
38.【知识迁移】二叉树的深度
	输入一棵二叉树，求该树的深度。从根结点到叶结点依次经过的结点（含根、叶结点）形成树的一条路径，最长路径的长度为树的深度。
	```
	def TreeDepth(self, pRoot):
        if not pRoot:
            return 0
        a=[pRoot]
        d=0
        while a:
            b=[]
            for node in a:
                if node.left:
                    b.append(node.left)
                if node.right:
                    b.append(node.right)
            a=b
            d=d+1
        return d
	```
39.【知识迁移】平衡二叉树
	输入一棵二叉树，判断该二叉树是否是平衡二叉树。
	```
	class Solution:
        def IsBalanced_Solution(self, pRoot):
            return self.TreeDepth(pRoot) !=-1
        def TreeDepth(self,root):
            if not root:
                return 0
            left = self.TreeDepth(root.left)
            if left == -1:
                return -1
            right = self.TreeDepth(root.right)
            if right == -1:
                return -1
            return -1 if abs(left-right)>1 else max(left,right)+1
	```
40.【知识迁移】数组中只出现一次的数字
	一个整型数组里除了两个数字之外，其他的数字都出现了两次。请写程序找出这两个只出现一次的数字。
	```
	def FindNumsAppearOnce(self, array):
        aset = set()
        for i in array:
            if i in aset:
                aset.remove(i)
            else:
                aset.add(i)
        return list(aset)
	```
41.【知识迁移】和为S的连续正数序列
	小明很喜欢数学,有一天他在做数学作业时,要求计算出9~16的和,他马上就写出了正确答案是100。但是他并不满足于此,他在想究竟有多少种连续的正数序列的和为100(至少包括两个数)。没多久,他就得到另一组连续正数和为100的序列:18,19,20,21,22。现在把问题交给你,你能不能也很快的找出所有和为S的连续正数序列? Good Luck!
	```
	def FindContinuousSequence(self, tsum):
        small = 1
        big = 2
        res = small+big
        reslist = []
        mid = (tsum+1)>>1
        while small<mid:
            if res == tsum:
                reslist.append(range(small,big+1)) #
                big += 1 #
                res += big #
            elif res<tsum:
                big += 1
                res += big
            else:
                res -= small
                small += 1
        return reslist
    ```
42.【知识迁移】和为S的两个数字
	```
	def FindNumbersWithSum(self, array, tsum):
        small = 0
        big = len(array)-1      
        reslist = []
        while (small+1)<=big:
            res = array[small]+array[big]
            if res == tsum:
                reslist.append([array[small],array[big]])
                big -= 1
            elif res < tsum:
                small += 1
            else:
                big -= 1
        if reslist==[]:
            return []
        else :
            return reslist[0]
	```
43.【知识迁移】左旋转字符串
	汇编语言中有一种移位指令叫做循环左移（ROL），现在有个简单的任务，就是用字符串模拟这个指令的运算结果。对于一个给定的字符序列S，请你把其循环左移K位后的序列输出。例如，字符序列S=”abcXYZdef”,要求输出循环左移3位后的结果，即“XYZdefabc”。是不是很简单？OK，搞定它！
	```
	class Solution:
        def reverse(self,s):
            length = len(s)
            if not s:
                return ''
            if length<=0:
                return ''
            s = list(s)
            start = 0
            end = length-1
            while start<end:
                s[start],s[end]= s[end],s[start]
                start += 1
                end -= 1
            return ''.join(s)
        def LeftRotateString(self, s, n):
            # write code here
            length = len(s)
            if length<=0:
                return ''
            if n>length:
                n = n%length
            first = ''
            second = ''
            for i in range(n):
                first += s[i]
            for i in range(n,length):
                second += s[i]
            first = self.reverse(first)
            second = self.reverse(second)
            return self.reverse(first+second)
	```
44.【知识迁移】翻转单词顺序列
	牛客最近来了一个新员工Fish，每天早晨总是会拿着一本英文杂志，写些句子在本子上。同事Cat对Fish写的内容颇感兴趣，有一天他向Fish借来翻看，但却读不懂它的意思。例如，“student. a am I”。后来才意识到，这家伙原来把句子单词的顺序翻转了，正确的句子应该是“I am a student.”。Cat对一一的翻转这些单词顺序可不在行，你能帮助他么？
	```
	class Solution:
        def reverse(self,s,start,end):
            if not s:
                return ''
            length = len(s)
            if length<=0:
                return ''
            s = list(s) #必须要有,不然不能使用s[i]=s[j]
            while start<end:
                s[start],s[end]= s[end],s[start]
                start += 1
                end -= 1
            return ''.join(s)
        def ReverseSentence(self, s):
            # write code here
            length = len(s)
            if length <=0:
                return ''
            #整体翻转
            s = self.reverse(s,0,length-1)
            #局部翻转
            st= 0
            e = 0
            i = 0
            while i<length:
                while i<length and s[i] == ' ':
                    i += 1
                st = i #记录非空格的第一个字符的位置
                e = i
                while i<length and s[i]!=' ':
                    i += 1
                    e += 1
                s = self.reverse(s,st,e-1)
            return s
	```
45.【抽象建模】扑克牌顺子
	LL今天心情特别好,因为他去买了一副扑克牌,发现里面居然有2个大王,2个小王(一副牌原本是54张^_^)...他随机从中抽出了5张牌,想测测自己的手气,看看能不能抽到顺子,如果抽到的话,他决定去买体育彩票,嘿嘿！！“红心A,黑桃3,小王,大王,方片5”,“Oh My God!”不是顺子.....LL不高兴了,他想了想,决定大\小 王可以看成任何数字,并且A看作1,J为11,Q为12,K为13。上面的5张牌就可以变成“1,2,3,4,5”(大小王分别看作2和4),“So Lucky!”。LL决定去买体育彩票啦。 现在,要求你使用这幅牌模拟上面的过程,然后告诉我们LL的运气如何， 如果牌能组成顺子就输出true，否则就输出false。为了方便起见,你可以认为大小王是0。
	```
	class Solution:
        def IsContinuous(self, numbers):
            # write code here
            if numbers == None or numbers==[]:
                return False
            zeros = 0
            gaps = 0
            mlen = len(numbers)
            numbers = self.qsort(numbers,0,mlen-1)
            for i in numbers:
                if i == 0 :
                    zeros += 1
                if i>0:
                    break
            n1,n2 = [zeros,zeros+1]
            while n2<mlen:
                if numbers[n1] == numbers[n2]:
                    return False
                gaps += numbers[n2]-numbers[n1]-1
                n1 = n2
                n2 += 1
            return True if zeros>=gaps else False
        def qsort(self,numbers,left,right):
            if left<right:
                i = left
                j = right
                base = numbers[i]
                while i<j:
                    while i<j and numbers[j]>=base:
                        j -= 1
                    numbers[i] = numbers[j]
                    while i<j and numbers[i]<base:
                        i += 1
                    numbers[j] = numbers[i]
                numbers[i] = base
                self.qsort(numbers,left,i-1)
                self.qsort(numbers,i+1,right)
            return numbers
	```
46.【抽象建模】孩子们的游戏(圆圈中最后剩下的数)
	每年六一儿童节,牛客都会准备一些小礼物去看望孤儿院的小朋友,今年亦是如此。HF作为牛客的资深元老,自然也准备了一些小游戏。其中,有个游戏是这样的:首先,让小朋友们围成一个大圈。然后,他随机指定一个数m,让编号为0的小朋友开始报数。每次喊到m-1的那个小朋友要出列唱首歌,然后可以在礼品箱中任意的挑选礼物,并且不再回到圈中,从他的下一个小朋友开始,继续0...m-1报数....这样下去....直到剩下最后一个小朋友,可以不用表演,并且拿到牛客名贵的“名侦探柯南”典藏版(名额有限哦!!^_^)。请你试着想下,哪个小朋友会得到这份礼品呢？(注：小朋友的编号是从0到n-1)
	```
	def LastRemaining_Solution(self, n, m):
        if n<1 or m<1:
            return -1
        last = 0
        for i in range(2,n+1):
            last = (last+m)%i
        return last
	```
47.【发散思维】求1+2+3+...+n
	```
	def Sum_Solution(self, n):
        return n and n+self.Sum_Solution(n-1)
	```
48.【发散思维】不用加减乘除做加法
	```
	def Add(self, num1, num2):
        while num2 != 0:
            temp = num1 ^ num2
            num2 = (num1 & num2) << 1
            # 为后面的移位操作做准备
            num1 = temp & 0xFFFFFFFF
        # 防止溢出
        return num1 if num1 >> 31 == 0 else num1 - 4294967296
	```
49.【综合】把字符串转换成整数
	```
	import re
	import math
    class Solution:
        def StrToInt(self, s):
            # write code here
            mlen = len(s)
            res = 0
            # 字符串非法
            if s=="0" or s=='' or re.search(r'[a-zA-Z]',s):
                return 0
            elif s[0]=='-': #字符串带-号
                for i in range(1,mlen):
                    res += int(s[i])*math.pow(10,mlen-i-1)
                return (-1)*res #正数变负数
            elif s[0]=="+": #字符串带+号
                for i in range(1,mlen):
                    res += int(s[i])*math.pow(10,mlen-i-1)
                return res
            else: #不带符号
                for i in range(0,mlen):
                    res += int(s[i])*math.pow(10,mlen-i-1)
                return res
	```
50.【数组】数组中重复的数字
	```
	class Solution:
    # 这里要特别注意~找到任意重复的一个值并赋值到duplication[0]
    # 函数返回True/False
    def duplicate(self, numbers, duplication):
        # write code here
        mlen = len(numbers)
    #设置列表，存储是否存在，因为规定输入的数值范围为（0,n-1），所以设置mlen个标志。
        icon = [False]*mlen
        for i in range(mlen):
            # 若数值超过（0,n-1）,忽略不计
            if numbers[i]>=mlen:
                break
            if icon[numbers[i]]==True:
                duplication[0]=numbers[i]
                return True
            icon[numbers[i]]=True
        return False
	```
51.【数组】	构建乘积数组
	给定一个数组A[0,1,...,n-1],请构建一个数组B[0,1,...,n-1],其中B中的元素B[i]=A[0]*A[1]*...*A[i-1]*A[i+1]*...*A[n-1]。不能使用除法。
	```
	def multiply(self, A):
        if not A:
            return []
        B = A[:]
        index = 0
        mlen = len(A)
        for i in range(mlen):
            sum = 1
            index = i
            # 计算A的连乘积，连乘除A[i]外的所有元素
            for j in range(mlen):
                if j!=index:
                    sum *= A[j]
            B[index] = sum
        return B
	```
52.【字符串】正则表达式匹配
	请实现一个函数用来匹配包括'.'和'*'的正则表达式。模式中的字符'.'表示任意一个字符，而'*'表示它前面的字符可以出现任意次（包含0次）。 在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串"aaa"与模式"a.a"和"ab*ac*a"匹配，但是与"aa.a"和"ab*a"均不匹配
	```
	def match(self, s, pattern):
        if len(s)==0 and len(pattern)==0:
            return True
        if len(s)>0 and len(pattern)==0:
            return False
        # 当模式中的第二个字符是“*”时
        if len(pattern)>1 and pattern[1]=="*":
        #如果字符串第一个模式跟模式第一个字符匹配（相等或匹配到“.”），可以有3种匹配方式
            if len(s)>0 and (s[0]==pattern[0] or pattern[0]=='.'):
            #1.模式后移2字符，相当于x*被忽略
            #2.字符串后移1字符，模式后移2字符
            #3.字符串后移1字符，模式不变，即继续匹配字符下一位，因为*可以匹配多位
                return self.match(s,pattern[2:]) or self.match(s[1:],pattern[2:]) or self.match(s[1:],pattern)
            else:
                return self.match(s,pattern[2:])
        if len(s)>0 and (s[0]==pattern[0] or pattern[0]=='.'):
            return self.match(s[1:],pattern[1:])
        return False
	```
53.【字符串】表示数值的字符串
	请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100","5e2","-123","3.1416"和"-1E-16"都表示数值。 但是"12e","1a3.14","1.2.3","+-5"和"12e+4.3"都不是。
	```
	import re
    class Solution:
        # s字符串
        def isNumeric(self, s):
            # write code here
            return re.match(r"^[\+\-]?[0-9]*(\.[0-9]*)?([eE][\+\-]?[0-9]+)?$",s)
	```
54.【字符串】字符流中第一个不重复的字符
	请实现一个函数用来找出字符流中第一个只出现一次的字符。例如，当从字符流中只读出前两个字符"go"时，第一个只出现一次的字符是"g"。当从该字符流中读出前六个字符“google"时，第一个只出现一次的字符是"l"。
	```
	class Solution:
        chars = []
        counts = []
        def __init__(self):
            self.chars = []
            self.counts = []
        def FirstAppearingOnce(self):
            # write code here
            counter = 0
            while counter<len(self.counts):
                if self.counts[counter]==1:
                    return self.chars[counter]
                counter += 1
            return "#"
        def Insert(self, char):
            # write code here
            if char in self.chars:
                self.counts[self.chars.index(char)] += 1
            else:
                self.chars.append(char)
                self.counts.append(1)
	```
55.【链表】链表中环的入口结点
	```
	def EntryNodeOfLoop(self, pHead):
        if pHead == None or pHead.next == None:
            return None
        tempList = []
        p = pHead
        while p:
            if p in tempList :
                return p
            tempList.append(p)
            p = p.next
        return None

	```
56.【链表】删除链表中重复的结点
	```
	def deleteDuplication(self, pHead):
        mlist = []
        p = pHead
        while p:
            mlist.append(p.val)
            p = p.next
        mhead = ListNode(None) # new link head
        chead = mhead  # new node to save the current point
        for i in mlist:
            if mlist.count(i) == 1:
                chead.next = ListNode(i)
                chead = chead.next
        return mhead.next
	```
57.【树】二叉树的下一个结点
	```
	def GetNext(self, pNode):
        if not pNode:
            return None
        # 若该结点有右子树，返回右子树的最左边结点
        if (pNode.right!=None):
            pNode = pNode.right
            while (pNode.left!=None):
                pNode = pNode.left
            return pNode
        # 若该结点无右子树，即为左孩子/右孩子。
        while (pNode.next!=None):
            # 若该结点为左孩子，返回其父结点。
            if pNode.next.left==pNode:
                return pNode.next
            # 若该结点为右孩子，返回其父结点的父结点
            pNode = pNode.next
	```
58.【树】对称的二叉树
	```
	class Solution:
        def isSymmetrical(self, pRoot):
            # write code here
            if not pRoot:
                return True
            return self.compare(pRoot.left,pRoot.right)
        def compare(self,left,right):
            if left==None:
                return right==None
            if right==None:
                return False
            if left.val!=right.val:
                return False
            return self.compare(left.left,right.right) and self.compare(left.right,right.left)
	```
59.【树】按之字形顺序打印二叉树
	```
	def Print(self, pRoot):
        if not pRoot:
            return []
        from collections import deque
        res,tmp=[],[]
        last=pRoot
        q=deque([pRoot])
        left_to_right=True
        while q:
            t=q.popleft()
            tmp.append(t.val)
            if t.left:
                q.append(t.left)
            if t.right:
                q.append(t.right)
            if t == last:          
                res.append(tmp if left_to_right else tmp[::-1])
                left_to_right= not left_to_right
                tmp=[]
                if q:last=q[-1]
        return res
	```
60.【树】把二叉树打印成多行
	```
	class Solution:
        # 返回二维列表[[1,2],[4,5]]
        def Print(self, pRoot):
            # write code here
            if not pRoot:
                return []
            res=[]
            tmp=[pRoot]
            while tmp:
                size=len(tmp)
                row=[]
                for i in tmp:
                    row.append(i.val)
                res.append(row)
                for i in range(size):
                    node=tmp.pop(0)
                    if node.left:
                        tmp.append(node.left)
                    if node.right:
                        tmp.append(node.right)
            return res
	```
61.【树】序列化二叉树
	二叉树的序列化是指：把一棵二叉树按照某种遍历方式的结果以某种格式保存为字符串，从而使得内存中建立起来的二叉树可以持久保存。序列化可以基于先序、中序、后序、层序的二叉树遍历方式来进行修改，序列化的结果是一个字符串，序列化时通过 某种符号表示空节点（井号），以 ！ 表示一个结点值的结束（value!）。二叉树的反序列化是指：根据某种遍历顺序得到的序列化字符串结果str，重构二叉树。
	```
	class Solution:
        flag = -1
        def Serialize(self, root):
            # write code here
            s = ''
            s = self.recSerialize(root,s)
            return s
        def recSerialize(self,root,s):
            if not root:
                return '$,'
            s = str(root.val)+","
            left = self.recSerialize(root.left,s)
            right = self.recSerialize(root.right,s)
            s += left+right
            return s
        def Deserialize(self, s):
            # write code here
            # 通过flag控制序列访问的下标
            self.flag += 1
            l = s.split(',')
            if (self.flag>=len(s)):
                return None
            root = None
            if (l[self.flag]!='$'):
                root = TreeNode(int(l[self.flag]))
                root.left = self.Deserialize(s)
                root.right = self.Deserialize(s)
            return root
	```
62.【树】二叉搜索树的第k个结点
	```
	class Solution:
        # 返回对应节点TreeNode
        def KthNode(self, pRoot, k):
            # write code here
            global result
            result=[]
            self.midnode(pRoot)
            if  k<=0 or len(result)<k:
                return None
            else:
                return result[k-1]
        def midnode(self,root):
            if not root:
                return None
            self.midnode(root.left)
            result.append(root)
            self.midnode(root.right)
	```
63.【树】数据流中的中位数
	如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。我们使用Insert()方法读取数据流，使用GetMedian()方法获取当前读取数据的中位数。
	```
	class Solution:
        def __init__(self):
            self.data=[]
        def Insert(self, num):
            # write code here
            self.data.append(num)
            self.data.sort()
        def GetMedian(self,data):
            # write code here
            length=len(self.data)
            if length%2==0:
                return (self.data[length//2]+self.data[length//2-1])/2.0
            else:
                return self.data[int(length//2)]
	```
64.【栈和队列】滑动窗口的最大值
	给定一个数组和滑动窗口的大小，找出所有滑动窗口里数值的最大值。例如，如果输入数组{2,3,4,2,6,2,5,1}及滑动窗口的大小3，那么一共存在6个滑动窗口，他们的最大值分别为{4,4,6,6,6,5}； 针对数组{2,3,4,2,6,2,5,1}的滑动窗口有以下6个： {[2,3,4],2,6,2,5,1}， {2,[3,4,2],6,2,5,1}， {2,3,[4,2,6],2,5,1}， {2,3,4,[2,6,2],5,1}， {2,3,4,2,[6,2,5],1}， {2,3,4,2,6,[2,5,1]}。
	```
	from collections import deque
	class Solution:
        def maxInWindows(self, num, size):
            # write code here
            if not num or size==0:
                return []
            d = deque()
            res = []
            for i in range(len(num)):
                while(len(d) and num[d[-1]]<=num[i]):
                    d.pop()
                while(len(d) and i-d[0]+1>size):
                    d.popleft()
                d.append(i)
                if (i+1>=size):
                    res.append(num[d[0]])
            return res
	```
65.【回溯法】矩阵中的路径
	请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一个格子开始，每一步可以在矩阵中向左，向右，向上，向下移动一个格子。如果一条路径经过了矩阵中的某一个格子，则该路径不能再进入该格子。 例如 a b c e s f c s a d e e 矩阵中包含一条字符串"bcced"的路径，但是矩阵中不包含"abcb"路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入该格子。

66.【回溯法】机器人的运动范围
	地上有一个m行和n列的方格。一个机器人从坐标0,0的格子开始移动，每一次只能向左，右，上，下四个方向移动一格，但是不能进入行坐标和列坐标的数位之和大于k的格子。 例如，当k为18时，机器人能够进入方格（35,37），因为3+5+3+7 = 18。但是，它不能进入方格（35,38），因为3+5+3+8 = 19。请问该机器人能够达到多少个格子？

67.【动态规划与贪婪】剪绳子
	给你一根长度为n的绳子，请把绳子剪成m段（m、n都是整数，n>1并且m>1），每段绳子的长度记为k[0],k[1],...,k[m]。请问k[0]xk[1]x...xk[m]可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

